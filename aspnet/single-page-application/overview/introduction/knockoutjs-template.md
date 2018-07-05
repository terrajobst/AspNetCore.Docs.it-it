---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Applicazione a pagina singola: Modello Knockout. js | Microsoft Docs'
author: MikeWasson
description: Modello Knockout
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 9e48c7033cefe8b6e4c456577b7a6448f9815d0b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832529"
---
<a name="single-page-application-knockoutjs-template"></a>Applicazione a pagina singola: Modello Knockout. js
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Il modello MVC Knockout fa parte di ASP.NET and Web Tools 2012.2
> 
> [Download di ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


L'aggiornamento ASP.NET and Web Tools 2012.2 include un modello di applicazione a pagina singola (SPA) per ASP.NET MVC 4. Questo modello è progettato per iniziare a creare rapidamente App web lato client interattivo.

"Applicazione a singola pagina" (SPA) è il termine generale per un'applicazione web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine. Dopo il caricamento della pagina iniziale, l'applicazione a singola pagina comunica con il server tramite le richieste AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX è nulla di nuovo, ma oggi esistono Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni. Inoltre, HTML5 e CSS3 vengono semplificando la creazione di interfacce utente avanzate.

Per iniziare a usare, il modello di applicazione a singola pagina Crea un'applicazione di esempio "To Do list". In questa esercitazione prenderemo in una presentazione del modello. Si verrà innanzitutto esaminare l'applicazione di elenco attività da eseguire se stesso, quindi esaminare le parti di tecnologia per il funzionamento.

## <a name="create-a-new-spa-template-project"></a>Creare un nuovo progetto di modello di applicazione a singola pagina

Requisiti:

- Visual Studio 2012 o Visual Studio Express 2012 per Web
- ASP.NET Web Tools 2012.2 update. È possibile installare l'aggiornamento [qui](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Avviare Visual Studio e selezionare **nuovo progetto** dalla pagina iniziale. E viceversa, il **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

![](knockoutjs-template/_static/image2.png)

Nel **nuovo progetto** procedura guidata, selezionare **applicazione a pagina singola**.

![](knockoutjs-template/_static/image3.png)

Premere F5 per compilare ed eseguire l'applicazione. Quando l'applicazione viene eseguita prima di tutto, viene visualizzata una schermata di accesso.

![](knockoutjs-template/_static/image4.png)

Scegliere il &quot;iscriversi&quot; collegare e creare un nuovo utente.

![](knockoutjs-template/_static/image5.png)

Dopo l'accesso, l'applicazione crea un elenco di attività predefinito con due elementi. È possibile fare clic su "Add Todo list" per aggiungere un nuovo elenco.

![](knockoutjs-template/_static/image6.png)

Rinominare l'elenco, aggiungere elementi all'elenco e deselezionarle. È anche possibile eliminare gli elementi o eliminare un intero elenco. Le modifiche vengono rese automaticamente persistenti in un database nel server (in realtà LocalDB a questo punto, poiché l'applicazione viene eseguita in locale).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architettura del modello di applicazione a singola pagina

Questo diagramma illustra i blocchi predefiniti principali per l'applicazione.

![](knockoutjs-template/_static/image8.png)

Sul lato server, ASP.NET MVC viene usato il codice HTML e gestisce anche l'autenticazione basata su form.

API Web ASP.NET gestisce tutte le richieste che si tratti della ToDoLists di elementi ToDoItems, tra cui introduzione, creazione, aggiornamento ed eliminazione. Il client scambia dati con l'API Web in formato JSON.

Entity Framework (EF) è il livello di O/RM. Funge da intermediario tra il mondo orientate a oggetti di ASP.NET e il database sottostante. Il database utilizza LocalDB, ma è possibile modificarlo nel file Web. config. In genere si potrebbe usare LocalDB per lo sviluppo locale e quindi distribuire in un database SQL nel server, utilizzando la migrazione code first di Entity Framework.

Sul lato client, la libreria Knockout. js gestisce gli aggiornamenti a pagina dalle richieste AJAX. Knockout utilizza l'associazione dati per sincronizzare la pagina con i dati più recenti. In questo modo, non devi scrivere codice che illustra in modo dettagliato i dati JSON e aggiorna il DOM. È invece inserire attributi dichiarativi nel codice HTML che indicano Knockout come presentare i dati.

Un grande vantaggio di questa architettura è che lo separa il livello di presentazione dalla logica dell'applicazione. È possibile creare la parte di API Web senza dover conoscere l'aspetto della pagina web. Sul lato client, si crea un "modello di visualizzazione" per rappresentare i dati e il modello di visualizzazione Usa Knockout da associare al codice HTML. Che consente di modificare facilmente il codice HTML senza modificare il modello di visualizzazione. (Esamineremo Knockout un po' più tardi.)

## <a name="models"></a>Modelli

Nel progetto di Visual Studio, la cartella Models contiene i modelli utilizzati sul lato server. (Sono disponibili anche modelli sul lato client; torniamo tra breve a quelli).

![](knockoutjs-template/_static/image9.png)

**Elemento TodoItem, TodoList**

Questi sono i modelli di database per Code First di Entity Framework. Si noti che questi modelli dispongono di proprietà che puntano a vicenda. `ToDoList` contiene una raccolta di elementi ToDoItems e ogni `ToDoItem` dispone di un riferimento al relativo elemento padre ToDoList. Queste proprietà sono definite proprietà di navigazione e la relazione uno-a-molti che rappresentano un elenco attività e i relativi elementi di attività da eseguire.

Il `ToDoItem` utilizzato anche dalla classe la **[perché ForeignKey]** attributo per specificare che `ToDoListId` è una chiave esterna nel `ToDoList` tabella. Indica a Entity Framework per aggiungere un vincolo di chiave esterna nel database.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Queste classi definiscono i dati che verranno inviati al client. "DTO" è l'acronimo di "data transfer object". Quest'ultimo definisce come l'entità verrà serializzata in JSON. In generale, esistono diversi motivi per usare oggetti DTO:

- Per controllare le proprietà che vengono serializzate. Quest'ultimo può contenere un subset delle proprietà dal modello di dominio. È possibile eseguire questa operazione per motivi di sicurezza (nascondere i dati sensibili) o semplicemente per ridurre la quantità di dati inviati.
- Per modificare la forma dei dati, ad esempio, rendere flat una struttura di dati più complessa.
- Per mantenere tutte le regole di business all'esterno di quest'ultimo (la separazione dei compiti).
- Se i modelli di dominio non possono essere serializzati per qualche motivo. Ad esempio, i riferimenti circolari possono causare problemi quando si serializza un oggetto esistono modi per gestire questo problema in API Web (vedere [gestisce i riferimenti all'oggetto circolare](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ma è semplicemente usando un oggetto DTO possibile evitare completamente il problema.

Nel modello di applicazione a singola pagina, agli oggetti DTO contiene gli stessi dati come modelli di dominio. Tuttavia, sono comunque utili poiché essi evitare riferimenti circolari dalle proprietà di navigazione e illustrano il modello oggetto DTO generale.

**AccountModels.cs**

Questo file contiene modelli per l'appartenenza al sito. Il `UserProfile` classe definisce lo schema per i profili utente nel database di appartenenza. (In questo caso, l'unica informazione è l'ID utente e il nome utente.) Le altre classi di modello in questo file vengono utilizzate per creare l'utente di moduli di registrazione e account di accesso.

## <a name="entity-framework"></a>Entity Framework

Il modello di applicazione a singola pagina utilizza Code First di Entity Framework. Nell'ambiente di sviluppo Code First definire innanzitutto i modelli nel codice e quindi Entity Framework Usa il modello per creare il database. È anche possibile usare EF con un database esistente ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

Il `TodoItemContext` deriva dalla classe nella cartella Models **DbContext**. Questa classe fornisce la "glue" tra i modelli ed Entity Framework. Il `TodoItemContext` contiene un `ToDoItem` raccolta e un `TodoList` raccolta. Per eseguire query sul database, è sufficiente scrivere una query LINQ per queste raccolte. Ad esempio, ecco come è possibile selezionare tutti gli elenchi di attività da eseguire per l'utente "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

È anche possibile aggiungere nuovi elementi alla raccolta, aggiornare gli elementi, o eliminare elementi dalla raccolta e rendere permanenti le modifiche al database.

## <a name="aspnet-web-api-controllers"></a>Controller API Web ASP.NET

Nell'API Web ASP.NET, i controller sono gli oggetti che gestiscono le richieste HTTP. Come accennato, il modello di applicazione a singola pagina Usa l'API Web per attivare le operazioni CRUD `ToDoList` e `ToDoItem` istanze. I controller si trovano nella cartella controller della soluzione.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Gestisce le richieste HTTP per gli elementi attività
- `TodoListController`: Gestisce le richieste HTTP per gli elenchi di attività da eseguire.

Questi nomi sono significativi, perché l'API Web corrispondente al percorso URI per il nome del controller. (Per informazioni su come API Web instrada le richieste HTTP al controller, vedere [Routing in API Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Verrà ora esaminato il `ToDoListController` classe. Contiene un membro dati singolo:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Il `TodoItemContext` viene usato per comunicare con Entity Framework, come descritto in precedenza. I metodi nel controller di implementano le operazioni CRUD. API Web viene eseguito il mapping delle richieste HTTP dal client per i metodi controller, come indicato di seguito:

| Richiesta HTTP | Metodo del controller | Descrizione |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Ottiene una raccolta di elenchi di attività. |
| GET/API/todo/*id* | `GetTodoList` | Ottiene un elenco di cose da fare per ID |
| PUT/API/todo/*id* | `PutTodoList` | Aggiorna un elenco attività. |
| POST /api/todo | `PostTodoList` | Crea un nuovo elenco attività. |
| DELETE/API/todo/*id* | `DeleteTodoList` | Elimina un elenco di attività. |

Si noti che gli URI per alcune operazioni contengono segnaposto per il valore ID. Per eliminare un elenco per con un ID di 42, ad esempio, l'URI è `/api/todo/42`.

Per altre informazioni sull'uso di API Web per le operazioni CRUD, vedere [creazione di un'API Web che supporta le operazioni CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Il codice per questo controller è piuttosto semplice. Ecco alcuni punti interessanti:

- Il `GetTodoLists` metodo utilizza una query LINQ per filtrare i risultati in base all'ID dell'utente connesso. In questo modo, un utente visualizza solo i dati appartenenti al proprietario stesso. Si noti anche che un'istruzione Select viene utilizzata per convertire le `ToDoList` istanze in `TodoListDto` istanze.
- I metodi PUT e post-controllano lo stato del modello prima di modificare il database. Se **ModelState** è false, questi metodi restituiscono HTTP 400, richiesta non valida. Altre informazioni sulla convalida del modello in API Web in [convalida dei modelli](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- La classe controller è anche decorata con il **[Authorize]** attributo. Questo attributo controlla se la richiesta HTTP è autenticata. Se la richiesta non è autenticata, il client riceve HTTP 401, non autorizzato. Altre informazioni su autenticazione al [autenticazione e autorizzazione nell'API Web ASP.NET](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

Il `TodoController` è molto simile alla classe `TodoListController`. La differenza principale è che non definisce metodi GET, in quanto il client otterrà gli elementi di attività e ogni elenco di attività da eseguire.

## <a name="mvc-controllers-and-views"></a>Viste e controller MVC

I controller MVC sono disponibili anche nella cartella controller della soluzione. `HomeController` esegue il rendering HTML principale per l'applicazione. La visualizzazione per il controller Home è definita in Views/Home/Index.cshtml. La visualizzazione iniziale viene eseguito il rendering di contenuto diverso a seconda del fatto che l'utente è connesso:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Quando gli utenti connessi, visualizzano l'interfaccia utente principale. In caso contrario, viene visualizzato il pannello di accesso. Si noti che il rendering condizionale venga eseguita sul lato server. Non tentare mai di nascondere i contenuti sensibili sul lato client & 8212anything & inviati in una risposta HTTP è visibile a un utente che controlla i messaggi HTTP non elaborati.

## <a name="client-side-javascript-and-knockoutjs"></a>Knockout. js e JavaScript sul lato client

A questo punto è possibile concentrare sul lato server dell'applicazione al client. Il modello di applicazione a singola pagina Usa una combinazione di jQuery e Knockout. js per creare un'interfaccia utente semplice e interattiva. Knockout. js è una libreria JavaScript che rende più semplice associare HTML ai dati. Knockout. js Usa un modello denominato "Model-View-ViewModel."

- Il modello è i dati di dominio (elenchi di attività e gli elementi ToDo).
- La vista è il documento HTML.
- Il modello di visualizzazione è un oggetto JavaScript che contiene i dati del modello. Il modello di visualizzazione è un'astrazione di codice dell'interfaccia utente. Dispone di informazioni relative alla relativa rappresentazione in HTML. Rappresenta invece uno funzionalità astratta della visualizzazione, ad esempio "un elenco di elementi ToDo".

La vista è associato a dati al modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono automaticamente riflesse nella visualizzazione. Le associazioni funzionano l'altra direzione. Gli eventi nel DOM (ad esempio un clic) vengono associati a funzioni nel modello di visualizzazione che attivano le chiamate AJAX.

Il modello di applicazione a singola pagina consente di organizzare il codice JavaScript lato client in tre livelli:

- TODO.DataContext.js: invia le richieste AJAX.
- TODO.Model.js: definisce i modelli.
- TODO.ViewModel.js: definisce il modello di visualizzazione.

![](knockoutjs-template/_static/image11.png)

Questi file di script si trovano nella cartella della soluzione di script/app.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** gestisce tutte le chiamate AJAX al controller API Web. (Le chiamate AJAX per l'accesso sono definite altrove, ajaxlogin.js.)

**TODO.Model.js** definisce i modelli lato client (browser) per gli elenchi di attività da eseguire. Esistono due classi di modello: todoItem e todoList.

Molte delle proprietà nelle classi modello sono di tipo "ko.observable". Oggetti osservabili sono qual è la sua magia Knockout. Dal [documentazione Knockout](http://knockoutjs.com/documentation/introduction.html): un oggetto osservabile è un "oggetto JavaScript che può inviare una notifica sulle modifiche apportate gli abbonati." Quando viene modificato il valore di un oggetto osservabile, Knockout Aggiorna tutti gli elementi HTML associati a tali oggetti osservabili. Ad esempio, todoItem ha oggetti osservabili per le proprietà di titolo e l'operazione viene eseguita:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

È inoltre possibile sottoscrivere un oggetto osservabile nel codice. Ad esempio, la classe todoItem sottoscrive modifiche nelle proprietà del "operazione viene eseguita" e "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modello di visualizzazione**

Il modello di visualizzazione è definito in todo.viewmodel.js. Il modello di visualizzazione è il punto centrale in cui l'applicazione associa gli elementi della pagina HTML per i dati di dominio. Nel modello di applicazione a singola pagina, il modello di visualizzazione contiene una matrice osservabile di todoLists. Il codice seguente nel modello di visualizzazione indica Knockout per applicare i binding:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML e il Data Binding

Il codice HTML principale per la pagina è definito in Views/Home/Index.cshtml. Poiché si sta usando l'associazione dati, il codice HTML è solo un modello per ciò che effettivamente viene sottoposto a rendering. Usa Knockout *dichiarativa* associazioni. Gli elementi della pagina associare a dati mediante l'aggiunta di un attributo "data-bind" sull'elemento. Ecco un esempio molto semplice, tratto dalla documentazione Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

In questo esempio, Knockout aggiorna il contenuto del **&lt;span&gt;** elemento con il valore di `myItems.count()`. Ogni volta che questo valore viene modificato, Knockout aggiorna il documento.

Knockout fornisce una serie di diversi tipi di binding. Ecco alcune delle associazioni usate nel modello di applicazione a singola pagina:

- **foreach**: consente di eseguire l'iterazione attraverso un ciclo e applicare lo stesso markup per ogni elemento nell'elenco. Viene utilizzato per eseguire il rendering di elenchi di attività e gli elementi attività. All'interno di **foreach**, i binding vengono applicati agli elementi dell'elenco.
- **visibile**: usata per attivare o disattivare la visibilità. Nascondere i commenti quando una raccolta è vuota o rendere visibile il messaggio di errore.
- **valore**: usata per popolare i valori del form.
- **Fare clic su**: associa un evento click per una funzione nel modello di visualizzazione.

## <a name="anti-csrf-protection"></a>Protezione Intersito

Cross-Site richiesta intersito falsa (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso. Per evitare attacchi CSRF, ASP.NET MVC Usa *token antifalsificazione*, anche definita richiesta del token di verifica. L'idea è che il server viene inserito un token generato in modo casuale una pagina web. Quando il client invia dati al server, questo valore deve includere nel messaggio di richiesta.

I token antifalsificazione funzionano perché la pagina non è in grado di leggere i token dell'utente, a causa dei criteri di corrispondenza dell'origine. (Criteri di corrispondenza dell'origine prevenzione documenti ospitati in due siti diversi l'accesso al contenuto di altro).

ASP.NET MVC offre supporto incorporato per i token antifalsificazione, tramite il [anti falsificazione](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) classe e il [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) attributo. Attualmente, questa funzionalità non è incorporata in API Web. Tuttavia, il modello di applicazione a singola pagina include un'implementazione personalizzata per l'API Web. Questo codice è definito nel `ValidateHttpAntiForgeryTokenAttribute` (classe), che si trova nella cartella filtri della soluzione. Per altre informazioni sulle anti-richieste intersito false nelle API Web, vedere [impedendo Cross-Site Request Forgery (CSRF) Attacks](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusione

Il modello SPA è progettato per iniziare a usare rapidamente la scrittura di applicazioni web interattive moderne. Usa la libreria Knockout. js per separare presentazione (markup HTML) dalla logica di dati e applicazioni. Ma Knockout non è l'unica libreria JavaScript che è possibile usare per creare un'applicazione a singola pagina. Se si desidera esplorare alcune altre opzioni, esaminiamo il [modelli creati dalla community SPA](../templates/index.md).
