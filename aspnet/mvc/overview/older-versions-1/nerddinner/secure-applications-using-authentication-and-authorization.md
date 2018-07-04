---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteggere le applicazioni usando l'autenticazione e autorizzazione | Microsoft Docs
author: microsoft
description: Passaggio 9 viene illustrato come aggiungere l'autenticazione e autorizzazione per proteggere l'applicazione di NerdDinner, in modo che gli utenti devono registrarsi e accedere al sito da creare...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d28102c8b80433b58a42cadc70b26c9fb5bc4404
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369884"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Proteggere le applicazioni usando l'autenticazione e autorizzazione
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 9 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 9 viene illustrato come aggiungere l'autenticazione e autorizzazione per proteggere l'applicazione di NerdDinner, in modo che gli utenti devono registrarsi e accedere al sito di creare nuovo dinners e solo l'utente che ospita un dinner possibile modificarlo in un secondo momento.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner passaggio 9: Autenticazione e autorizzazione

Ora nostro NerdDinner applicazione concede tutti gli utenti visita il sito la possibilità di creare e modificare i dettagli di qualsiasi cena. Verrà modificato in modo che gli utenti devono registrare ed eseguire l'accesso al sito per creare nuovi dinners e aggiungere una restrizione in modo che solo l'utente che ospita un dinner possibile modificarlo in un secondo momento.

A tale scopo si userà l'autenticazione e autorizzazione per proteggere l'applicazione.

### <a name="understanding-authentication-and-authorization"></a>Informazioni sull'autenticazione e autorizzazione

*Autenticazione* è il processo di identificazione e la convalida dell'identità di un client che accede a un'applicazione. In parole più semplici, è sull'identificazione "che l'utente finale è che visitano un sito Web". ASP.NET supporta più modalità per autenticare gli utenti del browser. Per le applicazioni web Internet, l'approccio più comune di autenticazione utilizzato è denominato "Autenticazione basata su form". Autenticazione basata su form consente agli sviluppatori di creare un form di accesso HTML nelle loro applicazioni e quindi convalidare il nome utente/password che un utente finale invia su un database o un altro archivio delle credenziali password. Se la combinazione di nome utente/password è corretta, lo sviluppatore può chiedere ASP.NET per rilasciare un cookie HTTP crittografato per identificare l'utente in tutte le richieste future. Ti invieremo un tramite autenticazione basata su form con la nostra applicazione NerdDinner.

*Autorizzazione* è il processo volto a determinare se un utente autenticato dispone dell'autorizzazione per accedere a un particolare URL/risorsa o per eseguire un'azione. Ad esempio, all'interno dell'applicazione di NerdDinner è opportuno autorizzare che possono accedere solo gli utenti che hanno effettuati l'accesso nel *Dinners/crea* URL e creare nuovi Dinners. È anche opportuno aggiungere la logica di autorizzazione in modo che solo l'utente che ospita un dinner può modificarla, perché e negare l'accesso in modifica a tutti gli altri utenti.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticazione basata su form e il AccountController

Il modello di progetto di Visual Studio predefinito per ASP.NET MVC abilita automaticamente l'autenticazione basata su form quando vengono create nuove applicazioni ASP.NET MVC. Aggiunge automaticamente anche un'implementazione di pagina di accesso di account predefiniti per il progetto, che rende molto semplice integrare la protezione all'interno di un sito.

La pagina master Site. master predefinito viene visualizzato un collegamento "Accedi" in alto a destra del sito quando l'utente accede a tale colonna non è autenticata:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Facendo clic sul collegamento "Accedi" consente a un utente di */Account/LogOn* URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Visitatori che non hanno eseguito la registrazione possono farlo facendo clic sul collegamento "Register", che li indirizzerà al */Account/Register* URL e consentire loro di immettere i dettagli dell'account:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Facendo clic sul pulsante "Register" Crea un nuovo utente all'interno del sistema di appartenenze ASP.NET e autenticare l'utente al sito con autenticazione basata su form.

Quando un utente ha eseguito l'accesso, il Site. master cambia alto a destra della pagina per restituire un "benvenuto [username]" messaggio ed esegue il rendering di un "Disconnetti" collegamento anziché un "Accedi" uno. Facendo clic sul collegamento "Disconnetti" disconnette l'utente:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La funzionalità di accesso, disconnessione e la registrazione precedente viene implementata all'interno della classe AccountController che è stato aggiunto al progetto da Visual Studio durante la creazione del progetto. L'interfaccia utente per il AccountController viene implementata usando i modelli di visualizzazione all'interno della directory \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La classe AccountController Usa il sistema di autenticazione basata su form ASP.NET per rilasciare i cookie di autenticazione crittografato e l'API di appartenenza ASP.NET per archiviare e convalidare nomi utente e password. L'API di appartenenza ASP.NET è estendibile e consente a qualsiasi archivio di credenziali di password da utilizzare. ASP.NET viene fornito con implementazioni del provider di appartenenza incorporata che archiviano nome utente e password all'interno di un database SQL o all'interno di Active Directory.

È possibile configurare il provider di appartenenze utilizzino la nostra applicazione NerdDinner aprendo il file "config" nella radice del progetto e cercando il &lt;appartenenza&gt; sezione all'interno di esso. Registra il provider di appartenenze SQL aggiunto quando è stato creato il progetto Web. config predefinito e lo configura per usare una stringa di connessione denominata "ApplicationServices" per specificare il percorso del database.

La stringa di connessione predefinita "ApplicationServices" (che viene specificato all'interno di &lt;connectionStrings&gt; sezione del file Web. config) è configurato per usare SQL Express. Punta a un database di SQL Express denominato "ASPNETDB. File. MDF"sotto l'applicazione" App\_dati "directory. Se questo database non esiste la prima volta che l'API di appartenenza viene usata all'interno dell'applicazione, ASP.NET verrà automaticamente creato il database e il provisioning dello schema di database di appartenenza appropriata all'interno di esso:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Se invece di usare SQL Express Desideravamo utilizzare un'istanza completa di SQL Server (o connettersi a un database remoto), tutto quello che sarebbe necessario attività da eseguire consiste nell'aggiornare la stringa di connessione "ApplicationServices" all'interno del file Web. config e verificare che lo schema di appartenenza appropriata è stato aggiunto al database di che cui punta. È possibile eseguire il "aspnet\_regsql.exe" utilità all'interno della directory \Windows\Microsoft.NET\Framework\v2.0.50727\ per aggiungere lo schema appropriato per l'appartenenza e di altri servizi delle applicazioni ASP.NET a un database.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorizzazione URL Dinners/Create usando il filtro [Authorize]

Non dobbiamo scrivere codice per abilitare un'autenticazione sicura e l'implementazione di gestione di account per l'applicazione di NerdDinner. Gli utenti possono registrare i nuovi account con l'applicazione e account di accesso/disconnessione del sito.

Ora è possibile aggiungere logica di autorizzazione all'applicazione e usare lo stato di autenticazione e nome utente di visitatori per controllare cosa possono e non è possibile all'interno del sito. Iniziamo aggiungendo la logica di autorizzazione ai metodi di azione "Crea" della nostra classe DinnersController. In particolare, verrà richiesto che gli utenti che accedono i */Dinners/crea* URL necessario eseguire l'accesso. Se non si accede si sarà reindirizzarli alla pagina di accesso in modo che essi possono Accedi.

Implementazione di questa logica è piuttosto semplice. È sufficiente cose da fare è aggiungere un attributo di filtro [Authorize] per i metodi di azione Create in questo modo:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC supporta la possibilità di creare "filtri di azione" che possono essere utilizzati per implementare la logica riutilizzabile che può essere applicata in modo dichiarativo ai metodi di azione. Il filtro [Authorize] è uno dei filtri azione predefiniti forniti da ASP.NET MVC, e consente agli sviluppatori di applicare in modo dichiarativo le regole di autorizzazione per i metodi di azione e le classi controller.

Quando applicato senza parametri (come sopra) consente di applicare il filtro [Authorize] che l'utente che effettua la richiesta del metodo di azione deve essere effettuato l'accesso, e si verrà reindirizzati automaticamente il browser all'URL di accesso se non sono. Quando si esegue il reindirizzamento URL richiesto originariamente viene passato come argomento di stringa di query (ad esempio: / Account/LogOn? ReturnUrl = % 2fDinners % 2fCreate). Il AccountController quindi reindirizzerà l'utente all'URL richiesto originariamente dopo l'accesso.

Il filtro [Authorize] supporta facoltativamente la possibilità di specificare una proprietà "Users" o "Roles" che può essere usata per richiedere che l'utente sia nel registro e all'interno di un elenco di utenti consentiti o un membro del ruolo di sicurezza consentiti. Ad esempio, il codice riportato di seguito consente solo due utenti specifici, "scottgu" e "billg" accedere all'URL Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Nomi utente specifici all'interno del codice di incorporamento tende a essere tuttavia abbastanza non facile da gestire. Un approccio migliore consiste nel definire "ruoli" di livello superiore che il codice di verifica e quindi eseguire il mapping di utenti al ruolo usando un database o un sistema di active directory (abilitando l'elenco di mapping degli utenti effettivi da archiviare esternamente dal codice). ASP.NET include un'API di gestione di ruolo predefinite, nonché un set predefinito di provider di ruoli (comprese quelle relative a SQL e Active Directory) che consentono di eseguire il mapping di utente o il ruolo. È quindi possibile aggiornare il codice per consentire agli utenti all'interno di un ruolo specifico "admin" accedere all'URL Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Usando la proprietà User.Identity.Name durante la creazione di Dinners

È possibile recuperare il nome utente dell'utente attualmente connesso in una richiesta usando la proprietà User.Identity.Name esposta nella classe base Controller.

Versioni precedenti quando viene implementata la versione HTTP-POST nostro create () del metodo di azione avevamo impostati come hardcoded la proprietà "HostedBy" della cena in una stringa statica. È possibile ora aggiornare il codice per usare invece la proprietà User.Identity.Name, nonché aggiungere automaticamente una RSVP per l'host crea la cena:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Poiché è stato aggiunto un attributo [Authorize] al metodo Create (), ASP.NET MVC garantisce che il metodo di azione viene eseguita solo se è connesso l'utente potrebbe visitare l'URL Dinners/Create nel sito. Di conseguenza, il valore della proprietà User.Identity.Name contiene sempre un nome utente valido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Usando la proprietà User.Identity.Name quando si modificano Dinners

Questo punto, aggiungere una logica di autorizzazione che limita gli utenti in modo che possano modificare solo le proprietà di dinners che vengono anch'essi ospitano.

A tale scopo, si aggiungerà innanzitutto un metodo di supporto "IsHostedBy(username)" per l'oggetto Dinner (all'interno della classe parziale di Dinner.cs che abbiamo creato in precedenza). Questo metodo helper restituisce true o false a seconda del fatto che un nome utente fornito corrisponde alla proprietà HostedBy Dinner e incapsula la logica necessaria per eseguire un confronto tra stringhe tra maiuscole e minuscole di essi:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Si aggiungerà quindi un attributo [Authorize] ai metodi di azione Edit () all'interno della classe DinnersController. Ciò garantisce che gli utenti devono essere connesso richiesta un */Dinners/modifica / [id]* URL.

È quindi possibile aggiungere codice per i metodi di modifica che usa il metodo helper Dinner.IsHostedBy(username) per verificare che l'utente connesso corrisponda all'host la cena. Se l'utente non è l'host, verranno mostrare una visualizzazione "InvalidOwner" e terminare la richiesta. Il codice per eseguire questa operazione è la seguente:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

È quindi possibile fare doppio clic sulla directory \Views\Dinners e scegliere Aggiungi -&gt;consente di visualizzare il comando di menu per creare una nuova visualizzazione "InvalidOwner". È possibile popolarla con il messaggio di errore seguente:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

E ora quando un utente tenta di modificare un dinner che non posseduto, si otterrà un messaggio di errore:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

È possibile ripetere gli stessi passaggi per i metodi di azione Delete () all'interno di controller per bloccare l'autorizzazione per eliminare anche Dinners e assicurarsi che il moderatore di un Dinner possibile eliminarlo.

### <a name="showinghiding-edit-and-delete-links"></a>Modifica di mostrare/nascondere e collegamenti di eliminazione

Si sta il collegamento per il metodo di azione di modifica e l'eliminazione della nostra classe DinnersController dal nostro URL dettagli:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Attualmente vengono mostrati i collegamenti all'azione modifica ed eliminazione indipendentemente dal fatto il visitatore per l'URL di informazioni sia l'host della cena. Verrà modificato in modo che i collegamenti vengono visualizzati solo se l'utente visita è il proprietario della cena.

Il metodo di azione Details() nel nostro DinnersController recupera un oggetto Dinner e quindi lo passa come l'oggetto modello per il modello di visualizzazione:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

È possibile aggiornare il modello di visualizzazione per in modo condizionale mostrare/nascondere i collegamenti di modifica ed eliminazione usando il Dinner.IsHostedBy() come il metodo helper seguente:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Passaggi successivi

Ora esaminiamo come è possibile abilitare gli utenti autenticati per conferma la tua partecipazione dinners tramite AJAX.

> [!div class="step-by-step"]
> [Precedente](implement-efficient-data-paging.md)
> [Successivo](use-ajax-to-deliver-dynamic-updates.md)
