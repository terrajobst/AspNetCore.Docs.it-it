---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Applicazione a pagina singola: Modello Knockout.js | Documenti Microsoft'
author: MikeWasson
description: Modello di ritaglio
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 6e84dcc16345e33fcd3a3f83c4b35bc993c03ca6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="single-page-application-knockoutjs-template"></a>Applicazione a pagina singola: Modello Knockout.js
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Il modello MVC Knockout fa parte di ASP.NET e Web strumenti 2012.2
> 
> [Scaricare ASP.NET e strumenti Web 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


L'aggiornamento ASP.NET e Web strumenti 2012.2 include un modello di applicazione a pagina singola (SPA) per ASP.NET MVC 4. Questo modello è progettato per iniziare a creare rapidamente applicazioni web sul lato client interattive.

"L'applicazione a pagina singola" (SPA) è il termine generale per un'applicazione web che carica una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine. Dopo il caricamento della pagina iniziale, l'autenticazione SPA comunica con il server tramite le richieste AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX è un concetto nuovo, ma esistono oggi Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni. Inoltre, HTML5 e CSS3 sono rendendo più semplice creare interfacce utente avanzate.

Per iniziare, il modello SPA crea un'applicazione di esempio "Elenco di attività". In questa esercitazione, prenderemo in una presentazione del modello. Si verrà innanzitutto esaminare l'applicazione elenco attività da eseguire, quindi esaminare le parti di tecnologia per il funzionamento.

## <a name="create-a-new-spa-template-project"></a>Creare un nuovo progetto di modello SPA

Requisiti:

- Visual Studio 2012 o Visual Studio Express 2012 per Web
- Aggiornare gli strumenti Web di ASP.NET 2012.2. È possibile installare l'aggiornamento [qui](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Avviare Visual Studio e selezionare **nuovo progetto** dalla pagina iniziale. O dal **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

![](knockoutjs-template/_static/image2.png)

Nel **nuovo progetto** procedura guidata, selezionare **applicazione a pagina singola**.

![](knockoutjs-template/_static/image3.png)

Premere F5 per compilare ed eseguire l'applicazione. Quando l'applicazione esegue la prima volta, viene visualizzata una schermata di accesso.

![](knockoutjs-template/_static/image4.png)

Fare clic su di &quot;iscriversi&quot; collegare e creare un nuovo utente.

![](knockoutjs-template/_static/image5.png)

Dopo l'accesso, l'applicazione crea un elenco di attività predefinito con due elementi. È possibile fare clic su "Aggiungi Todo list" per aggiungere un nuovo elenco.

![](knockoutjs-template/_static/image6.png)

Rinomina l'elenco, aggiungere elementi all'elenco e deselezionarle. È anche possibile eliminare gli elementi o eliminare un intero elenco. Le modifiche vengono rese automaticamente persistenti in un database nel server (effettivamente LocalDB a questo punto, poiché l'applicazione viene eseguita in locale).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architettura del modello di SPA

Questo diagramma illustra i principali blocchi predefiniti per l'applicazione.

![](knockoutjs-template/_static/image8.png)

Sul lato server, ASP.NET MVC viene utilizzato il codice HTML e gestisce inoltre l'autenticazione basata su form.

API Web ASP.NET gestisce tutte le richieste correlate al ToDoLists e ToDoItems, inclusi il recupero, creazione, aggiornamento ed eliminazione. Il client scambia dati con l'API Web in formato JSON.

Entity Framework (EF) è il livello di O/RM. Funge da intermediario tra il mondo orientata agli oggetti di ASP.NET e il database sottostante. Il database utilizza LocalDB, ma è possibile modificarlo nel file Web. config. In genere si usano LocalDB per lo sviluppo locale e quindi distribuire a un database SQL Server, tramite la migrazione di codice prima di Entity Framework.

Sul lato client, la libreria Knockout.js gestisce gli aggiornamenti a pagina da richieste AJAX. Knockout utilizza l'associazione dati per sincronizzare la pagina con i dati più recenti. In questo modo, non è necessario scrivere codice che esamina i dati JSON e aggiorna il modello DOM. È invece possibile inserire attributi dichiarativi nel codice HTML che indicano come presentare i dati di ritaglio.

Un grande vantaggio di questa architettura è che separa il livello di presentazione dalla logica dell'applicazione. È possibile creare la parte dell'API Web senza conoscere l'aspetto della pagina web nulla. Sul lato client, si crea un modello di visualizzazione"" per rappresentare i dati e il modello di visualizzazione utilizza Knockout per associare il codice HTML. Che consente di modificare facilmente il codice HTML senza modificare il modello di visualizzazione. (Verrà esaminato Knockout un bit in un secondo momento.)

## <a name="models"></a>Modelli

Nel progetto di Visual Studio, la cartella Modelli contiene i modelli utilizzati sul lato server. (Sul lato client sono anche i modelli, verranno fornite a quelli.)

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Questi sono i modelli di database per Entity Framework Code First. Si noti che i modelli hanno proprietà che fanno riferimento a altro. `ToDoList`contiene una raccolta di ToDoItems e ogni `ToDoItem` dispone di un riferimento al relativo elemento padre ToDoList. Queste proprietà sono definite proprietà di navigazione e rappresentano la relazione uno-a-molti, un elenco di attività e i relativi elementi di attività da eseguire.

Il `ToDoItem` utilizzato anche dalla classe di **[ForeignKey]** attributo per specificare che `ToDoListId` è una chiave esterna nel `ToDoList` tabella. Ciò indica a Entity Framework per aggiungere un vincolo di chiave esterna nel database.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Queste classi definiscono i dati che verranno inviati al client. "DTO" è l'acronimo di "oggetto di trasferimento di dati". Il DTO definisce come le entità verranno serializzate in JSON. In generale, esistono diversi motivi per utilizzare DTO:

- Per controllare le proprietà che vengono serializzate. Il DTO può contenere un sottoinsieme delle proprietà dal modello di dominio. È possibile farlo per motivi di sicurezza (nascondere i dati sensibili) o semplicemente per ridurre la quantità di dati da inviare.
- Per modificare la forma dei dati, ad esempio, per trasformare una struttura di dati più complessa.
- Per mantenere tutte le regole di business DTO (la separazione dei compiti).
- Se i modelli di dominio non possono essere serializzati per qualche motivo. Ad esempio, i riferimenti circolari possono causare problemi quando si serializza un oggetto sono disponibili modi per gestire questo problema nell'API Web (vedere [la gestione di riferimenti circolari oggetto](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ma utilizzando un DTO è semplicemente possibile evitare il problema del tutto.

Nel modello di SPA il DTO contiene gli stessi dati come modelli di dominio. Tuttavia, sono comunque utile poiché essi evitare riferimenti circolari le proprietà di navigazione e illustrano il modello generale di DTO.

**AccountModels.cs**

Questo file contiene i modelli per l'appartenenza al sito. La `UserProfile` classe definisce lo schema per i profili utente nelle appartenenze al database. (In questo caso, l'unica informazione è l'ID utente e il nome utente.) Le altre classi di modello in questo file vengono utilizzate per creare l'utente di moduli di registrazione e l'account di accesso.

## <a name="entity-framework"></a>Entity Framework

Il modello SPA utilizza Entity Framework Code First. Nello sviluppo Code First, definire i modelli innanzitutto nel codice e quindi utilizza il modello di Entity Framework per creare il database. È inoltre possibile utilizzare EF con un database esistente ([Database First](https://msdn.microsoft.com/en-us/data/jj206878.aspx)).

Il `TodoItemContext` classe nella cartella Models deriva da **DbContext**. Questa classe fornisce il "associazione" tra i modelli e di Entity Framework. Il `TodoItemContext` contiene un `ToDoItem` insieme e un `TodoList` insieme. Per eseguire query sul database, è sufficiente scrivere una query LINQ per queste raccolte. Ad esempio, ecco come è possibile selezionare tutti gli elenchi di attività da eseguire per l'utente "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

È anche possibile aggiungere nuovi elementi alla raccolta, aggiornare gli elementi, o eliminare elementi dalla raccolta e rendere permanenti le modifiche al database.

## <a name="aspnet-web-api-controllers"></a>Controller API Web ASP.NET

In ASP.NET Web API, i controller sono gli oggetti che gestiscono le richieste HTTP. Come indicato, il modello SPA utilizza l'API Web per abilitare le operazioni CRUD su `ToDoList` e `ToDoItem` istanze. Il controller si trovano nella cartella Controllers della soluzione.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Gestisce le richieste HTTP per gli elementi di attività da eseguire
- `TodoListController`: Gestisce le richieste HTTP per gli elenchi di attività da eseguire.

Questi nomi sono significativi, in quanto l'API Web corrispondente al percorso URI per il nome del controller. (Per informazioni su come API Web indirizza le richieste HTTP al controller, vedere [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Verrà ora esaminata la `ToDoListController` classe. Contiene un membro dati singolo:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Il `TodoItemContext` viene utilizzato per comunicare con Entity Framework, come descritto in precedenza. I metodi nel controller di implementano le operazioni CRUD. API Web esegue il mapping di richieste HTTP dal client per i metodi del controller, come indicato di seguito:

| Richiesta HTTP | Metodo del controller | Descrizione |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Ottiene una raccolta di elenchi di attività. |
| GET/api/attività/*id* | `GetTodoList` | Ottiene un elenco di attività da ID |
| PUT/api/attività/*id* | `PutTodoList` | Aggiorna un elenco di attività. |
| POST /api/todo | `PostTodoList` | Crea un nuovo elenco di attività. |
| DELETE/api/attività/*id* | `DeleteTodoList` | Elimina un elenco di attività. |

Si noti che gli URI per alcune operazioni contengono segnaposto per il valore ID. Per eliminare un elenco di con un ID di 42, ad esempio, l'URI è `/api/todo/42`.

Per ulteriori informazioni sull'utilizzo di API Web per le operazioni CRUD, vedere [la creazione di un'API Web che supporta le operazioni CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Il codice per questo controller è piuttosto semplice. Ecco alcuni aspetti interessanti:

- Il `GetTodoLists` metodo utilizza una query LINQ per filtrare i risultati in base all'ID dell'utente connesso. In questo modo, un utente visualizza solo i dati che appartiene all'utente. Si noti inoltre che un'istruzione Select viene usata per convertire il `ToDoList` istanze in `TodoListDto` istanze.
- I metodi PUT e POST controllano lo stato del modello prima di modificare il database. Se **ModelState.IsValid** è false, questi metodi restituiscono HTTP 400, richiesta non valida. Ulteriori informazioni sulla convalida del modello nell'API Web all'indirizzo [la convalida del modello](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- La classe controller anche è decorata con il **[Authorize]** attributo. Questo attributo controlla se la richiesta HTTP è stata autenticata. Se la richiesta non è autenticata, il client riceve HTTP 401, non autorizzato. Altre informazioni su autenticazione [autenticazione e autorizzazione in ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

Il `TodoController` è molto simile alla classe `TodoListController`. La differenza principale è che non definisce metodi GET, in quanto il client riceverà gli elementi di attività da eseguire con ogni elenco di attività da eseguire.

## <a name="mvc-controllers-and-views"></a>Le visualizzazioni e controller MVC

I controller MVC si trovano anche nella cartella Controllers della soluzione. `HomeController`esegue il rendering HTML per l'applicazione principale. La visualizzazione per il controller Home è definita in Views/Home/Index.cshtml. La visualizzazione iniziale viene eseguito il rendering di contenuto diverso a seconda se l'utente è connesso:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Quando gli utenti connessi, viene visualizzato l'interfaccia utente principale. In caso contrario, viene visualizzato il pannello di accesso. Si noti che il rendering condizionale si verifica sul lato server. Non tentare mai di nascondere il contenuto riservato sul lato client & #8212anything inviato nella risposta HTTP è visibile a un utente che controlla i messaggi HTTP non elaborati.

## <a name="client-side-javascript-and-knockoutjs"></a>Knockout.js e JavaScript sul lato client

Ora passiamo dal lato server dell'applicazione al client. Il modello SPA utilizza una combinazione di jQuery e Knockout.js per creare un'interfaccia utente interattiva uniforme. Knockout.js è una libreria JavaScript che rende più semplice associare HTML ai dati. Knockout.js utilizza un modello denominato "Model-View-ViewModel".

- Il modello è di tipo di dati del dominio (elenchi di attività e attività).
- La vista è il documento HTML.
- Il modello di visualizzazione è un oggetto JavaScript che contiene i dati del modello. Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente. Ha alcuna conoscenza della rappresentazione HTML. Rappresenta invece astratta funzionalità della visualizzazione, ad esempio "un elenco delle attività".

La vista con associazione dati per il modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono applicati automaticamente nella vista. Le associazioni funzionano nella direzione opposta anche. Gli eventi nel DOM (ad esempio un clic) sono dati associati a funzioni al modello di visualizzazione, che attivano le chiamate AJAX

Il modello SPA organizza JavaScript sul lato client in tre livelli:

- TODO.DataContext.js: invia le richieste AJAX.
- TODO.Model.js: definisce i modelli.
- TODO.ViewModel.js: definisce il modello di visualizzazione.

![](knockoutjs-template/_static/image11.png)

Questi file di script si trovano nella cartella della soluzione di script o app.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** gestisce tutte le chiamate AJAX per i controller API Web. (Le chiamate AJAX per l'accesso sono definite altrove, ajaxlogin.js.)

**TODO.Model.js** definisce i modelli sul lato client (browser) per gli elenchi di attività da eseguire. Esistono due classi di modello: todoItem e todoList.

Molte delle proprietà nelle classi modello sono di tipo "ko.observable". Oggetti osservabili sono come Knockout esegue l'operazione. Dal [documentazione Knockout](http://knockoutjs.com/documentation/introduction.html): observable è un "oggetto JavaScript che può inviare una notifica di sottoscrittori sulle modifiche." Quando viene modificato il valore di un oggetto observable, Knockout Aggiorna elementi HTML associati a tali oggetti osservabili. Ad esempio, todoItem ha Observable per le proprietà del titolo e isDone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

È inoltre possibile sottoscrivere un observable nel codice. Ad esempio, la classe todoItem sottoscrive modifiche nelle proprietà del "isDone" e "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modello di visualizzazione**

Il modello di visualizzazione è definito in todo.viewmodel.js. Il modello di visualizzazione è il punto centrale in cui l'applicazione associa gli elementi della pagina HTML per i dati di dominio. Nel modello di SPA, il modello di visualizzazione contiene una matrice observable di todoLists. Il codice seguente nel modello di visualizzazione indica Knockout per applicare i binding:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML e il Data Binding

Il codice HTML principale per la pagina è definito in Views/Home/Index.cshtml. Poiché si sta usando l'associazione dati, il codice HTML è solo un modello per ciò che effettivamente il rendering. Usa Knockout *dichiarativa* associazioni. Per associare elementi di pagina ai dati, aggiungendo un attributo "data-bind" all'elemento. Di seguito è riportato un esempio molto semplice, adottato nella documentazione di ritaglio:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

In questo esempio, Knockout aggiorna il contenuto del  **&lt;span&gt;**  elemento con il valore di `myItems.count()`. Ogni volta che questo valore viene modificato, Knockout aggiorna il documento.

Knockout fornisce una serie di tipi di associazione diversi. Di seguito sono riportate alcune delle associazioni utilizzate nel modello di SPA:

- **foreach**: consente di scorrere un ciclo e applicare gli stessi tag a ogni elemento nell'elenco. Viene utilizzato per eseguire il rendering di elenchi di attività e attività da svolgere. All'interno di **foreach**, i binding vengono applicati agli elementi dell'elenco.
- **visibile**: utilizzato per attivare o disattivare la visibilità. Nascondere i commenti quando una raccolta è vuota o rendere visibile il messaggio di errore.
- **valore**: utilizzato per popolare i valori del form.
- **Fare clic su**: associa un evento click per una funzione nel modello di visualizzazione.

## <a name="anti-csrf-protection"></a>Protezione anti-CSRF

Cross-Site richiesta Forgery (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso. Per impedire gli attacchi CSRF, MVC ASP.NET utilizza *i token antifalsificazione*, detta anche richiesta di token di verifica. L'idea è che il server viene inserito un token generato in modo casuale una pagina web. Quando il client invia dati al server, questo valore deve includere nel messaggio di richiesta.

I token antifalsificazione funzionano perché la pagina non è possibile leggere i token dell'utente, a causa di criteri di origine stesso. (Criteri stessa origine impediscono documenti ospitati in due diversi siti di accedere a contenuto di un'altra).

ASP.NET MVC fornisce supporto incorporato per i token antifalsificazione, tramite il [AntiForgery](https://msdn.microsoft.com/en-us/library/system.web.helpers.antiforgery.aspx) classe e [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute.aspx) attributo. Attualmente, questa funzionalità non è incorporata nell'API Web. Tuttavia, il modello SPA include un'implementazione personalizzata per l'API Web. Questo codice è definito nel `ValidateHttpAntiForgeryTokenAttribute` (classe), che si trova nella cartella della soluzione di filtri. Per ulteriori informazioni sulla anti-CSRF nell'API Web, vedere [attacchi di prevenzione Cross-Site Request Forgery (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusione

Il modello SPA è progettato per iniziare rapidamente la scrittura di applicazioni web moderne e interattivo. La libreria Knockout.js viene utilizzata per separare la presentazione (markup HTML) dalla logica di dati e dell'applicazione. Ma Knockout non è la libreria JavaScript sola che è possibile utilizzare per creare un SPA. Se si desidera esplorare altre opzioni, esaminiamo il [modelli creati dalla community SPA](../templates/index.md).
