---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: "Novità di Web Form in ASP.NET 4.5 | Documenti Microsoft"
author: rick-anderson
description: La nuova versione di Web Form ASP.NET introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza utente quando si lavora con dati. Nelle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 23e38416dc294a1a07cb320cf5ab328fa036d1e8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Novità di Web Form in ASP.NET 4.5
====================
da [categorie Web Team](https://twitter.com/webcamps)

> La nuova versione di Web Form ASP.NET introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza utente quando si lavora con dati.
> 
> Nelle versioni precedenti di Web Form, quando si utilizza l'associazione dati per generare il valore di un membro dell'oggetto, le espressioni di associazione dati Bind () o Eval () è stato utilizzato. Nella nuova versione di ASP.NET, si è in grado di dichiarare il tipo di dati un controllo sta per essere associato a tramite una nuova proprietà ItemType. L'impostazione di questa proprietà consentirà di utilizzare una variabile fortemente tipizzata per la ricezione di tutti i vantaggi dell'esperienza di sviluppo di Visual Studio, quali IntelliSense, spostamento di membri e il controllo in fase di compilazione.
> 
> Con i controlli con associazione a dati, è ora possibile anche specificare i propri metodi personalizzati per la selezione, aggiornamento, eliminazione e inserimento di dati, semplificando l'interazione tra i controlli della pagina e la logica dell'applicazione. Inoltre, le funzionalità di associazione del modello sono stati aggiunti per ASP.NET, ovvero che è possibile eseguire il mapping di dati dalla pagina direttamente nei parametri di tipo di metodo.
> 
> Convalida dell'input utente deve inoltre essere più semplice con la versione più recente di Web Form. È ora possibile annotare le classi di modello con gli attributi di convalida dal **System.ComponentModel.DataAnnotations** richiesta che controlla tutto il sito e lo spazio dei nomi convalidare l'input dell'utente utilizzando tali informazioni. La convalida lato client in Web Form è ora integrata con jQuery, fornendo pulitura codice lato client e funzionalità di JavaScript non intrusiva.
> 
> Nell'area richiesta la convalida sono stati apportati miglioramenti per renderne più semplice disattivare la convalida della richiesta per parti specifiche delle applicazioni o leggere i dati di richiesta invalidato in modo selettivo.
> 
> Alcune sono stati apportati miglioramenti a Web Form controlli server per sfruttare i vantaggi delle nuove funzionalità di HTML5:
> 
> - La proprietà TextMode del controllo casella di testo è stata aggiornata per supportare i nuovi tipi di input HTML5 come messaggio di posta elettronica, datetime e così via.
> - Il controllo FileUpload ora supporta più caricamenti di file dai browser che supportano questa funzionalità HTML5.
> - I controlli di convalida ora il supporto convalida HTML5 gli elementi di input.
> - Nuovi elementi HTML5 con attributi che rappresentano un URL ora supportano runat =&quot;server&quot;. Di conseguenza, è possibile utilizzare le convenzioni ASP.NET nei percorsi URL, ad esempio il ~ (operatore) per rappresentare la radice dell'applicazione (ad esempio, &lt;video runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Il controllo UpdatePanel è stato risolto per supportare i campi di input di registrazione HTML5.
> 
> Nel portale di ASP.NET ufficiale, è possibile trovare ulteriori esempi delle nuove funzionalità in ASP.NET 4.5 WebForms: [novità in ASP.NET 4.5 e Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Utilizzare le espressioni di associazione di dati fortemente tipizzati
- Usare nuove funzionalità di associazione del modello Web Form
- Utilizzare i provider di valori per il mapping dei dati di pagina ai metodi di codice
- Utilizzare le annotazioni dei dati per la convalida dell'input utente
- Richiedere advange di convalida sul lato client unobstrusive con jQuery in Web Form
- Implementare la convalida richiesta granulari
- Implementare asincrona elaborazione della pagina nel Web Form

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

È necessario che gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**L'installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.

Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: con i frammenti di codice](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione include gli esercizi seguenti:

1. [Esercizio 1: Modello di associazione in Web Form ASP.NET](#Exercise1)
2. [Esercizio 2: Convalida dei dati](#Exercise2)
3. [Esercizio 3: Pagina asincrona, l'elaborazione in ASP.NET Web Form](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Esercizio 1: Modello di associazione in Web Form ASP.NET

La nuova versione di Web Form ASP.NET introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza quando si lavora con dati. In questa esercitazione, si verrà informazioni sui controlli di dati fortemente tipizzati e associazione del modello.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Attività 1: utilizzo di associazioni di dati fortemente tipizzati

In questa attività, si scoprirà nuovi fortemente tipizzata binding disponibili in ASP.NET 4.5.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex1-ModelBinding/Begin/** cartella.

    1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **Customers** pagina. Inserire un elenco non numerato nel controllo principale e includere un controllo repeater per elencare ogni cliente. Impostare il nome del ripetitore **customersRepeater** come illustrato nel codice seguente.

    Nelle versioni precedenti di Web Form, quando si utilizza l'associazione dati per generare il valore di un membro di un oggetto si è l'associazione dati, utilizzare un'espressione di associazione dati, insieme a una chiamata al metodo Eval, passando il nome del membro come stringa.

    In fase di esecuzione, queste chiamate a Eval usare la reflection per l'oggetto attualmente associato per leggere il valore del membro con il nome specificato e visualizzare il risultato nell'HTML. Questo approccio è molto semplice eseguire l'associazione dati contro dati arbitrari, unshaped.

    Sfortunatamente, si perdono molte delle funzionalità ottimale dei problemi in fase di sviluppo in Visual Studio, tra cui IntelliSense per i nomi dei membri, il supporto per la navigazione (ad esempio, Vai a definizione) e controllo in fase di compilazione.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Aprire il **Customers.aspx.cs** file.
4. Aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Nel **pagina\_carico** metodo, aggiungere il codice per popolare Ripetitore con l'elenco dei clienti.

    (- Frammento di codice *Web di origine dati di form Lab - Ex01 - associazione clienti*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La soluzione utilizza EntityFramework insieme CodeFirst per creare e accedere al database. Nel codice seguente, il customersRepeater è associata a una query materializzata che restituisce tutti i clienti dal database.
6. Premere **F5** per eseguire la soluzione e selezionare il **clienti** per vedere in azione repeater. Come la soluzione utilizza CodeFirst, il database verrà creato e popolato nell'istanza di SQL Express locale quando si esegue l'applicazione.

    ![Elencare i clienti con un ripetitore](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "elencare i clienti con un ripetitore")

    *Elencare i clienti con un ripetitore*

    > [!NOTE]
    > In Visual Studio 2012, IIS Express è il server di sviluppo Web predefinito.
7. Chiudere il browser e tornare a Visual Studio.
8. Sostituire l'implementazione per l'utilizzo di associazioni fortemente tipizzate. Aprire il **Customers** pagina e usare il nuovo **ItemType** attributo nel repeater per impostare il **cliente** tipo come tipo di associazione.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La proprietà ItemType consente di dichiarare il tipo di dati il controllo sta per essere associato a e consente di utilizzare fortemente tipizzata di associazione all'interno del controllo con associazione a dati.
9. Sostituire il contenuto con il codice seguente ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Uno svantaggio con metodi precedenti è che le chiamate a Eval () e BIND () sono ad associazione tardiva, vale a dire che si passi le stringhe per rappresentare i nomi delle proprietà. Ciò significa che non si ottiene Intellisense per i nomi dei membri, supporto per lo spostamento di codice (ad esempio, Vai a definizione), né verifica supporto in fase di compilazione.

    L'impostazione della proprietà ItemType fa sì che due nuove variabili deve essere generato nell'ambito delle espressioni di associazione di dati tipizzate: **elemento** e **BindItem**. È possibile utilizzare queste variabili fortemente tipizzate nelle espressioni di associazione di dati e ottenere i vantaggi per lo sviluppo di Visual Studio.

    Il &quot; **:** &quot; utilizzato nell'espressione verrà automaticamente la codifica HTML l'output per evitare problemi di sicurezza (ad esempio, il cross-site scripting gli attacchi). Questa notazione era disponibile partire da .NET 4 per la scrittura di risposta, ma ora è disponibile anche in espressioni di associazione dati.

    > [!NOTE]
    > Membro dell'elemento funziona per l'associazione unidirezionale. Se si desidera eseguire l'associazione bidirezionale utilizzare il **BindItem** membro.

    ![Supporto di IntelliSense nell'associazione fortemente tipizzato](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "supporto IntelliSense nell'associazione fortemente tipizzati")

    *Supporto di IntelliSense nell'associazione fortemente tipizzati*
10. Premere **F5** per eseguire la soluzione e passare alla pagina clienti per assicurarsi che le modifiche funzionino come previsto.

    ![Visualizzazione Dettagli cliente](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Elenca i dettagli del cliente")

    *Elenca i dettagli del cliente*
11. Chiudere il browser e tornare a Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Attività 2 - Introduzione modello Web Form di associazione

Nelle versioni precedenti di Web Form ASP.NET, quando si desidera eseguire l'associazione dati bidirezionale, sia il recupero e l'aggiornamento dei dati, è necessario utilizzare un oggetto origine dati. Potrebbe trattarsi di un'origine dati oggetto, un'origine dati SQL, un'origine dati LINQ e così via. Tuttavia se lo scenario è necessario codice personalizzato per la gestione dei dati, è necessario utilizzare l'origine dati dell'oggetto e questo portato alcuni svantaggi. È necessario gestire le eccezioni quando si esegue la logica di convalida, ad esempio, è necessario evitare di tipi complessi.

Nella nuova versione di Web Form ASP.NET i controlli con associazione a dati supportano l'associazione di modelli. Ciò significa che è possibile specificare selezionare, aggiornare, inserire ed eliminare i metodi direttamente nel controllo con associazione a dati per chiamare la logica dal file code-behind o da un'altra classe.

Per ulteriori informazioni su questo, si utilizzerà un GridView per visualizzare l'elenco di categorie di prodotti utilizzando il nuovo **SelectMethod** attributo. Questo attributo consente di specificare un metodo per recuperare i dati di GridView.

1. Aprire il **Products** pagina e includere un **GridView**. Come illustrato di seguito per utilizzare associazioni fortemente tipizzati e abilitare l'ordinamento e paging, configurare il controllo GridView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Usare il nuovo **SelectMethod** attributi per configurare il controllo GridView per chiamare un **GetCategories** metodo per selezionare i dati.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Aprire il **Products.aspx.cs** codice file e aggiungere le seguenti istruzioni using.

    (- Frammento di codice *Web gli spazi dei nomi di form Lab - Ex01 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Aggiungere un membro privato nel **prodotti** e assegnare una nuova istanza della **ProductsContext**. Questa proprietà archivia il contesto dati Entity Framework che consente di connettersi al database.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Creare un **GetCategories** metodo per recuperare l'elenco delle categorie di utilizzo di LINQ. La query includerà il **prodotti** proprietà in modo GridView consente di visualizzare la quantità di prodotti per ogni categoria. Si noti che il metodo restituisce un oggetto IQueryable non elaborato che rappresentano la query viene eseguita in un secondo momento il ciclo di vita di pagina.

    (- Frammento di codice *Web Form Lab - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Nelle versioni precedenti di Web Form ASP.NET, abilitazione ordinamento e paging utilizzando la logica di repository all'interno di un contesto di origine dati dell'oggetto, è necessario scrivere codice personalizzato e la ricezione di tutti i parametri necessari. A questo punto, come i metodi di associazione di dati possono restituire IQueryable e rappresenta una query ancora per l'esecuzione, ASP.NET può essere usato per modificare la query per aggiungere un ordinamento e i parametri di paging.
6. Premere **F5** per avviare il debug del sito e passare alla pagina prodotti. Si noterà che GridView viene popolato con le categorie restituite dal metodo GetCategories.

    ![Popolamento di un controllo GridView mediante associazione di modelli](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "un GridView con utilizzo di associazione di modelli di popolamento")

    *Popolamento di un controllo GridView mediante associazione di modelli*
7. Premere **MAIUSC**+**F5** interrompere il debug.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Attività 3 - provider di valori di associazione del modello

Associazione di modelli non solo consente di specificare metodi personalizzati per lavorare con i dati direttamente nel controllo con associazione a dati, ma è anche possibile eseguire il mapping dei dati dalla pagina in parametri da questi metodi. Il parametro di metodo, è possibile utilizzare gli attributi del provider di valore per specificare l'origine dati del valore. Ad esempio:

- Controlli della pagina
- Valori di stringa di query
- Visualizzare i dati
- Stato della sessione
- Cookie
- Dati del modulo registrato
- Stato di visualizzazione
- Provider di valori personalizzati sono supportati anche

Se si utilizza ASP.NET MVC 4, si noterà che il supporto di associazione del modello è simile. In effetti, queste funzionalità sono state eseguite da ASP.NET MVC e spostate nel **System. Web** assembly sia in grado di utilizzare tali in Web Form.

In questa attività si aggiornerà il GridView per filtrare i risultati dalla quantità di prodotti per ogni categoria, riceve il parametro di filtro con l'associazione di modelli.

1. Tornare al **Products** pagina.
2. Nella parte superiore di GridView, aggiungere un **etichetta** e **ComboBox** per selezionare il numero di prodotti per ogni categoria, come illustrato di seguito.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Aggiungere un **EmptyDataTemplate** a GridView per mostrare un messaggio quando nessuna categoria con il numero di prodotti selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Aprire il **Products.aspx.cs** code-behind e aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modificare il **GetCategories** per ricevere un numero intero **minProductsCount** argomento e filtrare i risultati restituiti. A tale scopo, sostituire il metodo con il codice seguente.

    (- Frammento di codice *Web Form Lab - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Il nuovo **[controllo]** attributo la **minProductsCount** argomento informerà ASP.NET il relativo valore deve essere popolato usando un controllo nella pagina. ASP.NET verrà cercare qualsiasi controllo che corrisponde a quello dell'argomento (minProductsCount) ed eseguire il mapping necessario e conversione per riempire il parametro con il valore del controllo.

    In alternativa, l'attributo fornisce un costruttore di overload che consente di specificare il controllo da cui ottenere il valore.

    > [!NOTE]
    > Uno degli obiettivi di funzionalità di associazione dati consiste nel ridurre la quantità di codice che deve essere scritto per l'interazione di pagina. Oltre a provider di valori [controllo], è possibile utilizzare altri provider di associazione di modelli nei parametri del metodo. L'introduzione di attività sono elencati alcuni di essi.
6. Premere **F5** per avviare il debug del sito e passare alla pagina prodotti. Selezionare un numero di prodotti nell'elenco a discesa e notare come GridView è aggiornato.

    ![Filtro GridView con un valore di elenco a discesa](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtro GridView con un valore di riepilogo")

    *Filtro GridView con un valore di riepilogo*
7. Terminare il debug.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Attività 4: associazione per il filtro del modello utilizzando

In questa attività si aggiungerà un secondo figlio GridView per visualizzare i prodotti appartenenti alla categoria selezionata.

1. Aprire il **Products** pagina e aggiornare le categorie di GridView per generare automaticamente il pulsante Seleziona.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Aggiungere un secondo **GridView** denominato **productsGrid** nella parte inferiore. Impostare il **ItemType** per **WebFormsLab.Model.Product**, **DataKeyNames** per **ProductId** e **SelectMethod**  a **GetProducts**. Impostare **AutoGenerateColumns** a **false** e aggiungere le colonne per ProductId, ProductName, descrizione e UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Aprire il **Products.aspx.cs** file code-behind. Implementare il **GetProducts** per ricevere l'ID di categoria dalla categoria GridView e filtrare i prodotti. Associazione del modello verrà impostato il valore del parametro utilizzando la riga selezionata nel **categoriesGrid**. Poiché il nome dell'argomento e il nome di controllo non corrispondono, è necessario specificare il nome del controllo nel provider di valori di controllo.

    (- Frammento di codice *Web Form Lab - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Questo approccio rende più semplice per unit test di questi metodi. In un contesto di unit test, in cui non è in esecuzione Web Form, l'attributo [controllo] non eseguirà alcuna azione specifica.
4. Aprire il **Products** pagina e individuare i prodotti GridView. Aggiornare i prodotti GridView per visualizzare un collegamento per la modifica del prodotto selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Aprire il **ProductDetails.aspx** pagina codice e sostituire il **SelectProduct** (metodo) con il codice seguente.

    (- Frammento di codice *Web Form Lab - Ex01 - SelectProduct metodo*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Si noti che il **[QueryString]** attributo viene utilizzato per riempire il parametro del metodo da un parametro di productId nella stringa di query.
6. Premere **F5** per avviare il debug del sito e passare alla pagina prodotti. Selezionare qualsiasi categoria GridView le categorie e i prodotti di GridView viene aggiornata.

    ![Con i prodotti della categoria selezionata](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "che mostra i prodotti della categoria selezionata")

    *Con i prodotti della categoria selezionata*
7. Fare clic su di **vista** collegamento su un prodotto per aprire la pagina ProductDetails.aspx.

    Si noti che la pagina sta recuperando il prodotto con SelectMethod utilizzando il parametro productId dalla stringa di query.

    ![Visualizzazione dei dettagli prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "visualizzando i dettagli del prodotto")

    *Visualizzazione dei dettagli del prodotto*

    > [!NOTE]
    > La possibilità di immettere una descrizione HTML verrà implementata nell'esercizio successivo.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Attività 5 - associazione per le operazioni di aggiornamento del modello utilizzando

Nell'attività precedente è stata utilizzata l'associazione di modelli principalmente di selezione dei dati, in questa attività si apprenderà come usare l'associazione di modelli nelle operazioni di aggiornamento.

È possibile aggiornare le categorie di GridView per consentire all'utente di aggiornare le categorie.

1. Aprire il **Products** pagina e aggiornare le categorie di GridView per generare automaticamente sul pulsante di modifica e utilizzare il nuovo **UpdateMethod** attributo per specificare un **UpdateCategory**per aggiornare l'elemento selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    L'attributo DataKeyNames in GridView definire quali sono i membri che identificano in modo univoco l'oggetto con associazione a modello e di conseguenza, quali sono i parametri, che il metodo update deve ricevere ad almeno.
2. Aprire il **Products.aspx.cs** file code-behind e implementare il **UpdateCategory** metodo. Il metodo deve ricevere l'ID di categoria per caricare la categoria corrente, popolare i valori dal controllo GridView. e quindi aggiornare la categoria.

    (- Frammento di codice *Web Form Lab - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Il nuovo **TryUpdateModel** metodo nella classe della pagina è responsabile del popolamento dell'oggetto modello utilizzando i valori dai controlli nella pagina. In questo caso, che sostituirà i valori aggiornati rispetto alla riga corrente di GridView viene modificato nel **categoria** oggetto.

    > [!NOTE]
    > L'esercizio successivo verrà illustrato l'utilizzo del ModelState.IsValid per convalidare i dati immessi dall'utente quando si modifica l'oggetto.
3. Esecuzione del sito e passare alla pagina prodotti. Modificare una categoria. Digitare un nuovo nome e quindi fare clic su **aggiornamento** per rendere permanenti le modifiche.

    ![La modifica di categorie](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Modifica categorie")

    *La modifica delle categorie*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Esercizio 2: Convalida dei dati

In questo esercizio verranno fornite informazioni sulle nuove funzionalità di convalida dei dati in ASP.NET 4.5. Estrae le nuove funzionalità di convalida non intrusiva di Web Form. Si utilizzeranno le annotazioni dei dati nelle classi del modello di applicazione per la convalida dell'input utente e, infine, si apprenderà come attivare o disattivare la convalida della richiesta sui singoli controlli in una pagina.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Attività 1: convalida non intrusiva

Form dati complessi inclusi validator tendono a generare una quantità eccessiva codice JavaScript in una pagina, che può rappresentare circa 60% del codice. Con la convalida non intrusiva è abilitata, il codice HTML sarà più semplice e più chiaro.

In questa sezione verrà abilitata la convalida non intrusiva ASP.NET per confrontare il codice HTML generato da entrambe le configurazioni.

1. Aprire **Visual Studio 2012** e aprire il **iniziare** soluzione si trova nel **Source\Ex2 Validation\Begin** cartella di questa esercitazione. In alternativa, è possibile continuare a lavorare in una soluzione esistente nell'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, in Esplora soluzioni, fare clic su di **WebFormsLab** progetto **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Premere **F5** per avviare l'applicazione web. Passare ai clienti di pagina e fare clic il **aggiungere un nuovo cliente** collegamento.
3. Nella pagina del browser e scegliere **Visualizza origine** opzione per aprire il codice HTML generato dall'applicazione.

    ![Visualizzazione codice HTML della pagina](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "che mostra il codice HTML della pagina")

    *Visualizzazione codice HTML della pagina*
4. Scorrere il codice sorgente della pagina e si noti che ASP.NET è inserito JavaScript codice e i dati validator nella pagina per eseguire le convalide e visualizzare l'elenco degli errori.

    ![Codice di convalida JavaScript nella pagina CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "codice di convalida JavaScript nella pagina CustomerDetails")

    *Codice di convalida JavaScript nella pagina CustomerDetails*
5. Chiudere il browser e tornare a Visual Studio.
6. A questo punto verrà abilitata la convalida non intrusiva. Aprire **Web. config** e individuare **ValidationSettings:UnobtrusiveValidationMode** chiave nel **AppSettings** sezione **.** Impostare il valore della chiave **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > È inoltre possibile impostare questa proprietà &quot; **pagina\_carico** &quot; evento nel caso che si desidera abilitare la convalida non intrusivo solo per alcune pagine.
7. Aprire **CustomerDetails.aspx** e premere **F5** per avviare l'applicazione Web.
8. Premere il tasto F12 per aprire Strumenti di sviluppo di Internet Explorer. Una volta aperto gli strumenti di sviluppo, selezionare la scheda di script. Selezionare **CustomerDetails.aspx** nel menu e prendere nota che gli script necessari per l'esecuzione di jQuery nella pagina sono stati caricati nel browser dal sito locale.

    ![Il caricamento di jQuery JavaScript file direttamente dal server IIS locale](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "jQuery JavaScript di caricamento file direttamente dal server IIS locale")

    *Caricare i file JavaScript jQuery direttamente dal server IIS locale*
9. Chiudere il browser per tornare a Visual Studio. Aprire il **Site. master** file nuovo e individuare il **ScriptManager**. Aggiungere l'attributo **EnableCdn** proprietà con il valore **True**. Questa operazione forzerà jQuery deve essere caricata dall'URL online, non dall'URL del sito locale.
10. Aprire **CustomerDetails.aspx** in Visual Studio. Premere il tasto F5 per eseguire il sito. Una volta aperto Internet Explorer, premere il tasto F12 per aprire gli strumenti di sviluppo. Selezionare il **Script** scheda e quindi esaminare l'elenco a discesa. Si noti che non vengono caricati i file JavaScript jQuery non è più da sito locale, ma piuttosto dalla rete CDN di jQuery online.

    ![Il caricamento di jQuery JavaScript file dalla rete CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "jQuery JavaScript di caricamento file dalla rete CDN")

    *Caricare i file JavaScript jQuery dalla rete CDN*
11. Aprire il codice di origine della pagina HTML utilizzando l'opzione di origine di visualizzazione nel browser. Si noti che abilitando la convalida non intrusiva ASP.NET ha sostituito il codice JavaScript inserito con dati - \*attributi.

    ![Codice di convalida non intrusivo](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "codice di convalida non intrusivo")

    *Codice di convalida non intrusivo*

    > [!NOTE]
    > In questo esempio, si è visto come una riepilogo con le annotazioni dei dati di convalida è stata semplificata per solo alcune HTML e JavaScript righe. In precedenza, senza la convalida non intrusiva, i controlli di convalida più aggiunti, maggiori saranno le dimensioni il codice di convalida JavaScript aumenterà.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Attività 2: convalida del modello con le annotazioni dei dati

ASP.NET 4.5 introduce convalida le annotazioni dei dati per Web Form. Anziché un controllo di convalida su ogni input, è ora possibile definire vincoli nelle classi del modello e utilizzarle in tutto l'applicazione web. In questa sezione si apprenderà come usare le annotazioni dei dati per la convalida di form di una nuova/modifica.

1. Aprire **CustomerDetail.aspx** pagina. Si noti che il cliente nome il secondo nome nel **EditItemTemplate** e **InsertItemTemplate** sezioni vengono convalidate utilizzando un controlli RequiredFieldValidator. Ogni servizio di convalida è associato a una determinata condizione, pertanto è necessario includere tutti i validator come condizioni da verificare.
2. Aggiungere le annotazioni dei dati per convalidare la classe modello Customer. Aprire **Customer.cs** classe il **modello** cartella e *decorare* ogni proprietà usando gli attributi di annotazione dei dati.

    (- Frammento di codice *Web Form le annotazioni dei dati Lab - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 ha esteso la raccolta di annotazioni di dati esistente. Queste sono alcune delle annotazioni dei dati è possibile utilizzare: [CreditCard], [Phone], [EmailAddress], [intervallo], [confronto], [Url], [FileExtensions], [Required], [chiave], [RegularExpression].
    > 
    > Alcuni esempi di utilizzo:
    > 
    > [chiave]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > È inoltre possibile definire messaggi di errore all'interno di ogni attributo.
3. Aprire **CustomerDetails.aspx** e rimuovere tutte le RequiredFieldvalidators per i campi nome e cognome nelle sezioni EditItemTemplate e InsertItemTemplate del controllo FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Un vantaggio dell'utilizzo delle annotazioni di dati consiste nel fatto che la logica di convalida non viene duplicata nelle pagine dell'applicazione. Si definiscono una volta nel modello e usarlo in tutte le pagine di applicazione che modificano i dati.
4. Aprire **CustomerDetails.aspx** code-behind e individuare il metodo SaveCustomer. Questo metodo viene chiamato durante l'inserimento di un nuovo cliente e riceve il parametro di clienti dai valori del controllo FormView. Quando il mapping tra i controlli della pagina e si verifica quando l'oggetto di parametro, ASP.NET verrà eseguita la convalida del modello rispetto a tutte le annotazioni di dati degli attributi e riempire il dizionario ModelState con gli errori rilevati, se presente.

    Solo il ModelState.IsValid restituirà true se tutti i campi nel modello sono validi dopo l'esecuzione della convalida.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Aggiungere un **ValidationSummary** controllo alla fine della pagina CustomerDetails per visualizzare l'elenco di errori del modello.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    Il **ShowModelStateErrors** è una nuova proprietà il controllo ValidationSummary controllo che, se impostato **true**, il controllo visualizzerà gli errori dal dizionario ModelState. Questi errori derivano da convalida le annotazioni dei dati.
6. Premere **F5** per eseguire l'applicazione Web. Completare il modulo con alcuni valori non corretti e fare clic su **salvare** per eseguire la convalida. Si noti il riepilogo nella parte inferiore degli errori.

    ![Convalida con le annotazioni dei dati](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "convalida con le annotazioni dei dati")

    *Convalida con le annotazioni dei dati*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Attività 3: gestione degli errori di Database personalizzati con ModelState

Nella versione precedente di Web Form, la gestione degli errori di database, ad esempio una stringa troppo lunga o una violazione di chiave univoca può comportare la generazione di eccezioni nel codice del repository e quindi la gestione delle eccezioni nel code-behind per visualizzare un errore. Per eseguire un'operazione relativamente semplice, è necessario un elevato livello di codice.

In Web Form 4.5, l'oggetto ModelState può essere utilizzato per visualizzare gli errori di pagina, dal modello o dal database, in modo coerente.

In questa attività si aggiungerà codice per gestire le eccezioni di database e visualizzare il messaggio appropriato per l'utente utilizzando l'oggetto ModelState correttamente.

1. Mentre l'applicazione è ancora in esecuzione, provare ad aggiornare il nome di una categoria utilizzando un valore duplicato.

    ![Aggiornamento di una categoria con un nome duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "l'aggiornamento di una categoria con un nome duplicato")

    *Aggiornamento di una categoria con un nome duplicato*

    Si noti che viene generata un'eccezione a causa di &quot;univoco&quot; vincolo del **CategoryName** colonna.

    ![Eccezione per i nomi di categoria duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "eccezione per i nomi di categoria duplicato")

    *Eccezione per i nomi di categoria duplicato*
2. Terminare il debug. Nel **Products.aspx.cs** file code-behind, aggiornare il **UpdateCategory** metodo per gestire le eccezioni generate dal database. Metodo SaveChanges () chiamata e l'aggiunta di un errore di **ModelState** oggetto.

    Il nuovo **TryUpdateModel** metodo aggiorna l'oggetto della categoria recuperato dal database utilizzando i dati del form forniti dall'utente.

    (- Frammento di codice *Web Form Lab - Ex02 - UpdateCategory Handle errori*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Idealmente, è necessario identificare la causa dei DbUpdateException e verificare se la causa principale è la violazione di un vincolo di chiave univoca.
3. Aprire **Products** e aggiungere un **ValidationSummary** controllo sotto le categorie di GridView per visualizzare l'elenco di errori del modello.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Esecuzione del sito e passare alla pagina prodotti. Provare ad aggiornare il nome di una categoria utilizzando un valore duplicato.

    Si noti che è stata gestita l'eccezione e viene visualizzato il messaggio di errore di **ValidationSummary** controllo.

    ![Categoria errore duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicato categoria errore")

    *Errore di categoria duplicato*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Attività 4: richiedere la convalida in Web Form ASP.NET 4.5

La funzionalità di convalida richiesta ASP.NET fornisce un certo livello di protezione predefinita da attacchi di cross-site scripting (XSS). Nelle versioni precedenti di ASP.NET, la convalida della richiesta è stata abilitata per impostazione predefinita e può essere disattivata solo per un'intera pagina. Con la nuova versione di Web Form ASP.NET è possibile ora disabilitare la convalida della richiesta per un singolo controllo, eseguire la convalida della richiesta lenta o accedere ai dati di richiesta non convalidati (prestare attenzione in caso contrario!).

1. Premere **Ctrl + F5** per avviare il sito senza eseguire il debug e passare alla pagina prodotti. Selezionare una categoria, quindi scegliere il **modifica** collegamento in uno qualsiasi dei prodotti.
2. Digitare una descrizione contenente contenuto potenzialmente dannoso, ad esempio, inclusi i tag HTML. Prendere nota dell'eccezione generata a causa la convalida della richiesta.

    ![Modifica di un prodotto con contenuto potenzialmente dannoso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "la modifica di un prodotto con contenuto potenzialmente dannoso")

    *Modifica di un prodotto con contenuto potenzialmente dannoso*

    ![Eccezione generata a causa di convalida della richiesta](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "eccezione generata a causa di convalida della richiesta")

    *Eccezione generata a causa di convalida della richiesta*
3. Chiudere la pagina e, in Visual Studio, premere **MAIUSC + F5** per arrestare il debug.
4. Aprire il **ProductDetails.aspx** pagina e individuare il **descrizione** casella di testo.
5. Aggiungere il nuovo **ValidateRequestMode** proprietà per la casella di testo e impostarne il valore su **disabilitato**.

    Il nuovo **ValidateRequestMode** attributo consente di disabilitare la convalida della richiesta in modo granulare per ogni controllo. Ciò è utile quando si desidera utilizzare un input che può ricevere il codice HTML, ma desidera mantenere la convalida per il resto della pagina.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Premere **F5** per eseguire l'applicazione web. Riaprire la pagina del prodotto di modifica e completare una descrizione del prodotto, inclusi i tag HTML. Si noti che è ora possibile aggiungere contenuto HTML e la descrizione.

    ![Richiedere la convalida disabilitata per la descrizione del prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "disabilitata per la descrizione del prodotto di convalida della richiesta")

    *Convalida della richiesta disabilitata per la descrizione del prodotto*

    > [!NOTE]
    > In un'applicazione di produzione, è consigliabile puro il codice HTML immesso dall'utente per assicurarsi che vengano immessi solo provvisoria tag HTML (ad esempio, vi sono non &lt;script&gt; tag). A tale scopo, è possibile utilizzare [libreria di protezione di Microsoft Web](https://www.nuget.org/packages/AntiXSS).
7. Modificare di nuovo il prodotto. Nel campo Nome digitare il codice HTML e fare clic su **salvare**. Si noti che la convalida richiesta è disabilitata solo per il campo di descrizione e il resto dei campi re ancora convalidato rispetto al contenuto potenzialmente pericoloso.

    ![Abilitato con il resto dei campi di convalida delle richieste](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "abilitata in altri campi di convalida della richiesta")

    *Convalida delle richieste abilitata nel resto dei campi*

    Web Form ASP.NET 4.5 include una nuova modalità di convalida richiesta per eseguire la convalida delle richieste in modo differito. Con la modalità di convalida della richiesta impostata su **4.5**, se una porzione di codice accede *Request. Form [&quot;chiave&quot;]*, trigger verrà di convalida richiesta del ASP.NET 4.5 solo la convalida delle richieste Per questo elemento specifico nella raccolta di form.

    Inoltre, ASP.NET 4.5 include ora le routine di codifica core dalla libreria di Anti-XSS Microsoft 4.0. L'Anti-XSS codifica routine sono implementate dalla nuova *AntiXssEncoder* tipo disponibile nel nuovo **System.Web.Security.AntiXss** dello spazio dei nomi. Con il **encoderType** parametro configurato per utilizzare *AntiXssEncoder*, tutto l'output automaticamente la codifica in ASP.NET viene utilizzata la nuova routine di codifica.
8. Convalida della richiesta 4.5 ASP.NET supporta inoltre l'accesso non convalidato di richiedere i dati. ASP.NET 4.5 aggiunge una nuova proprietà di raccolta per il **HttpRequest** oggetto denominato **Unvalidated**. Quando ci si sposta in **HttpRequest.Unvalidated** si ha accesso a tutti i componenti comuni di dati della richiesta, tra cui moduli, QueryStrings, i cookie, URL e così via.

    ![Oggetto Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated oggetto")

    *Oggetto Request.Unvalidated*

    > [!NOTE]
    > **Utilizzare la proprietà HttpRequest.Unvalidated con cautela.** Assicurarsi di che eseguire con attenzione la convalida personalizzata ai dati non elaborati richiesta per assicurarsi che pericoloso testo non è sottoposto a round trip e rendering inconsapevole!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Esercizio 3: Pagina asincrona, l'elaborazione in ASP.NET Web Form

In questo esercizio verranno introdotte nella nuova pagina asincrona, l'elaborazione delle funzionalità in Web Form ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Pagina per caricare e visualizzare le immagini di dettagli attività 1 - l'aggiornamento del prodotto

In questa attività, aggiornare la pagina dei dettagli del prodotto per consentire all'utente di specificare un URL dell'immagine per il prodotto e visualizzarlo nella visualizzazione sola lettura. Scaricando il file in modo sincrono, si creerà una copia locale dell'immagine specificata. Nell'attività successiva, è possibile aggiornare questa implementazione per consentire il funzionamento in modo asincrono.

1. Aprire **Visual Studio 2012** e caricare il **iniziare** soluzione si trova **Source\Ex3 Async\Begin** dalla cartella dell'ambiente di test. In alternativa, è possibile continuare a lavorare nella soluzione esistente da esercizi precedenti.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, in Esplora soluzioni, fare clic su di **WebFormsLab** del progetto e selezionare **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **ProductDetails.aspx** pagina di origine e aggiungere un campo in ItemTemplate del controllo FormView per mostrare l'immagine di prodotto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Aggiungere un campo per specificare l'URL dell'immagine EditTemplate del controllo FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Aprire il **ProductDetails.aspx.cs** codice file e aggiungere le direttive dello spazio dei nomi seguenti.

    (- Frammento di codice *Web gli spazi dei nomi di form Lab - Ex03 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Creare un **UpdateProductImage** metodo per archiviare immagini remote locale **immagini** cartella e aggiornare l'entità product con il nuovo valore immagine del percorso.

    (- Frammento di codice *Web Form Lab - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aggiornamento di **UpdateProduct** metodo da chiamare il **UpdateProductImage** metodo.

    (- Frammento di codice *Web Form Lab - Ex03 - UpdateProductImage chiamata*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Eseguire l'applicazione e provare a caricare un'immagine per un prodotto. Ad esempio, è possibile utilizzare l'URL dell'immagine seguente da Office Clip arte: [ [http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg)

    ![L'impostazione di un'immagine per un prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "l'impostazione di un'immagine per un prodotto")

    *L'impostazione di un'immagine per un prodotto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Attività 2: aggiunta di elaborazione alla pagina dei dettagli prodotto asincrona

In questa attività, aggiornare la pagina dei dettagli del prodotto per utilizzarlo in modo asincrono. Si procederà al miglioramento un'attività a esecuzione prolungata - processo di download dell'immagine - tramite l'elaborazione asincrona delle pagine ASP.NET 4.5.

Metodi asincroni nelle applicazioni web consente di ottimizzare il modo in cui vengono utilizzati pool di thread ASP.NET. In ASP.NET sono sono richiede un numero limitato di thread nel pool di thread per tenere conto, pertanto, quando tutti i thread sono occupati, ASP.NET Rifiuta nuove richieste, invia i messaggi di errore di applicazione e rende disponibile il sito.

Operazioni lunghe ed elaborate nel sito web sono ottimi candidati per la programmazione asincrona occupa il thread assegnato per un lungo periodo. Ciò include le richieste di esecuzione prolungata, pagine con un numero elevato di diversi elementi e le pagine che richiedono operazioni non in linea, tale query su un database o l'accesso a un server web esterno. Il vantaggio è che se si utilizzano metodi asincroni per queste operazioni, durante l'elaborazione della pagina, il thread è liberato e restituito al thread di pool di applicazioni e può essere usato per partecipare a una nuova richiesta di pagina. Di conseguenza, la pagina verrà avviata l'elaborazione in un thread dal pool di thread e potrebbe essere completata l'elaborazione in una diversa, una volta completata l'elaborazione asincrona.

1. Aprire il **ProductDetails.aspx** pagina. Aggiungere il **Async** attributo la **pagina** elemento e impostarlo su **true**. Questo attributo indica ad ASP.NET di implementare l'interfaccia IHttpAsyncHandler.


    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Aggiungere un'etichetta nella parte inferiore della pagina per visualizzare i dettagli dei thread di esecuzione della pagina.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Aprire la console di **ProductDetails.aspx.cs** e aggiungere le direttive dello spazio dei nomi seguenti.

    (- Frammento di codice *Web gli spazi dei nomi di form Lab - Ex03 - 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modificare il **UpdateProductImage** metodo per scaricare l'immagine con un'attività asincrona. Si sostituirà la **WebClient** **DownloadFile** metodo con il **DownloadFileTaskAsync** (metodo) e includere il **await** (parola chiave).

    (- Frammento di codice *Web Form Lab - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    Il RegisterAsyncTask registra una nuova attività asincrona pagina deve essere eseguito in un altro thread. Riceve un'espressione lambda con l'esecuzione di attività (t). Il **await** parola chiave nel **DownloadFileTaskAsync** metodo converte il resto del metodo in un callback che viene richiamato in modo asincrono dopo il **DownloadFileTaskAsync** completamento del metodo. ASP.NET verrà ripresa l'esecuzione del metodo per gestire automaticamente tutti originali valori delle richieste HTTP. Il nuovo modello di programmazione asincrono in .NET 4.5 consente di scrivere codice asincrono che è molto simile al codice sincrono e consentire al compilatore di gestire la complessità delle funzioni di callback o codice di continuazione.
    > [!NOTE]
    > RegisterAsyncTask e PageAsyncTask non era già disponibile dopo .NET 2.0. La parola chiave await nuove dal modello di programmazione asincrono .NET 4.5 e può essere usata con i nuovi metodi TaskAsync dall'oggetto .NET WebClient.
5. Aggiungere codice per visualizzare i thread in cui il codice di avvio e al termine dell'esecuzione. A tale scopo, aggiornare il **UpdateProductImage** (metodo) con il codice seguente.

    (- Frammento di codice *Web Form Lab - Ex03 - Mostra thread*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Aprire il sito web **Web. config** file. Aggiungere la variabile appSetting seguente.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Premere **F5** per eseguire l'applicazione e caricare un'immagine per il prodotto. Si noti l'ID di thread in cui il codice di inizio e fine potrebbe essere diverso. Questo avviene perché l'esecuzione di attività asincrone in un thread separato dal pool di thread ASP.NET. Al completamento dell'attività, ASP.NET inserisce l'attività in coda e assegna di thread disponibili.

    ![Scaricare un'immagine in modo asincrono](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "scaricare un'immagine in modo asincrono")

    *Scaricare un'immagine in modo asincrono*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione Azure seguente [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questa esercitazione pratica individuati e illustrati i concetti seguenti:

- Utilizzare le espressioni di associazione di dati fortemente tipizzati
- Usare nuove funzionalità di associazione del modello Web Form
- Utilizzare i provider di valori per il mapping dei dati di pagina ai metodi di codice
- Utilizzare le annotazioni dei dati per la convalida dell'input utente
- Richiedere advange di convalida sul lato client unobstrusive con jQuery in Web Form
- Implementare la convalida richiesta granulari
- Implementare asincrona elaborazione della pagina nel Web Form

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Azure SDK*&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

    ![Installa Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Installazione completata*
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

Questa appendice mostra come creare un nuovo sito web dal portale di Azure e pubblicare l'applicazione che è stato acquistato seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Attività 1 - Creazione di un nuovo sito Web dal portale di Azure

1. Passare al [il portale di gestione di Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico. È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "accedere al portale Windows Azure")

    *Accedere al portale*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Selezionare quindi **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Azure è l'host di un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa in Azure dall'esterno al portale. Non include i passaggi per configurare un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "creando un nuovo sito Web utilizzando Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna. Verificare il funzionamento del nuovo sito Web.

    ![Esplorazione per il nuovo sito web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "esplorazione per il nuovo sito web")

    *Esplorazione per il nuovo sito web*

    ![Sito Web in esecuzione](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione del sito web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "apertura delle pagine di gestione del sito web")

    *Aprire le pagine di gestione del sito Web*
7. Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web in Azure.

    ![Profilo di pubblicazione del sito web di download](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "profilo di pubblicazione del sito web di download")

    *Profilo di pubblicazione del sito Web di download*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web in Azure da Visual Studio.

    ![Salvare il file del profilo di pubblicazione](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "il salvataggio del profilo di pubblicazione")

    *Salvare il file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL. Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Azure in **database Sql** | **server** | **Dashboard del Server**. Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi. Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard del Server Database SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Dashboard del Server Database SQL")

    *Dashboard del Server Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) pulsante.

    ![Aggiunta indirizzo IP del Client](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.

    ![La pubblicazione dell'applicazione](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "la pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "l'importazione del profilo di pubblicazione")

    *L'importazione di profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Una volta completata la convalida, fare clic su **Avanti**.

    > [!NOTE]
    > Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.

    ![Convalida della connessione](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).

    ![Configurazione della distribuzione Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

    - Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.
    - In **nome utente** digitare il nome di accesso di amministratore di server.
    - In **Password** digitare la password dell'account di accesso amministratore server.
    - Digitare un nuovo nome di database.

    ![Configurazione di stringa di connessione di destinazione](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "configurazione stringa di connessione di destinazione")

    *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database fare clic su **Sì**.

    ![Creazione del database](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si utilizzerà per connettersi al Database SQL di Azure viene visualizzata all'interno di casella di testo di connessione predefinito. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **anteprima** pagina, fare clic su **pubblica**.

    ![Pubblicare l'applicazione web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "pubblicare l'applicazione web")

    *Pubblicare l'applicazione web*
9. Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1. Fare clic in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*
