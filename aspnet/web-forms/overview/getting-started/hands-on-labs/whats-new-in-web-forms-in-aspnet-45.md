---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: What ' s New in Web Form ASP.NET 4.5 | Microsoft Docs
author: rick-anderson
description: La nuova versione di Web Form ASP.NET introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza utente quando si lavora con i dati. Nelle versioni precedenti di...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: f36c50b64ed2363ba648297a1424b638bf9d4af5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830412"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>What ' s New in Web Form ASP.NET 4.5
====================
da [Camp Web Team](https://twitter.com/webcamps)

> La nuova versione di Web Form ASP.NET introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza utente quando si lavora con i dati.
> 
> Nelle versioni precedenti di Web Form, quando si usa l'associazione dati per generare il valore di un membro dell'oggetto, le espressioni di associazione dati Bind () o Eval () è stata usata. Nella nuova versione di ASP.NET, si è in grado di dichiarare quali tipi di dati un controllo sta per essere associato a tramite una nuova proprietà di tipo di elemento. Impostazione di questa proprietà verrà consentono di usare una variabile fortemente tipizzato per ricevere i vantaggi dell'esperienza di sviluppo Visual Studio, ad esempio IntelliSense, spostamento membro e il controllo in fase di compilazione.
> 
> Con i controlli con associazione a dati, è ora possibile anche specificare i propri metodi personalizzati per la selezione, l'aggiornamento, eliminazione e inserimento di dati, semplificando l'interazione tra i controlli di pagina e la logica dell'applicazione. Inoltre, le funzionalità di associazione del modello sono stati aggiunti per ASP.NET, che significa che è possibile eseguire il mapping dei dati dalla pagina direttamente nei parametri di tipo di metodo.
> 
> Convalida dell'input utente deve anche essere più semplice con la versione più recente dei moduli Web. È ora possibile annotare le classi del modello con gli attributi di convalida dal **System.ComponentModel.DataAnnotations** richiesta che controlla tutte del sito e lo spazio dei nomi convalidare l'input usando le informazioni dell'utente. La convalida lato client in Web Form è ora integrata con jQuery, fornendo più chiaro il codice lato client e le funzionalità di JavaScript non intrusiva.
> 
> Nell'area di convalida richiesta, sono stati apportati miglioramenti per renderne più semplice disattivare la richiesta di convalida relativi a parti specifiche delle applicazioni o leggere i dati della richiesta invalidato in modo selettivo.
> 
> Alcuni sono stati apportati miglioramenti a Web Form controlli server per sfruttare i vantaggi delle nuove funzionalità di HTML5:
> 
> - La proprietà TextMode su del controllo casella di testo è stata aggiornata per supportare i nuovi tipi di input HTML5 come messaggio di posta elettronica, datetime e così via.
> - Il controllo FileUpload supporta ora più caricamenti di file dai browser che supportano questa funzionalità HTML5.
> - Ora il supporto durante la convalida HTML5 gli elementi di input di controlli di convalida.
> - Nuovi elementi HTML5 con gli attributi che rappresentano un URL ora supportano runat =&quot;server&quot;. Di conseguenza, è possibile usare le convenzioni di ASP.NET nei percorsi di URL, ad esempio il ~ operatore per rappresentare la radice dell'applicazione (ad esempio, &lt;video runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Il controllo UpdatePanel è stato risolto per supportare i campi di input di registrazione HTML5.
> 
> Nel portale di ASP.NET ufficiale è possibile trovare altri esempi delle nuove funzionalità in Web Form ASP.NET 4.5: [What ' s New in ASP.NET 4.5 e Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Usare le espressioni di associazione dati fortemente tipizzati
- Le nuove funzionalità di associazione del modello di Web Form
- Usare i provider di valori per il mapping di dati della pagina per i metodi code-behind
- Usare le annotazioni dei dati per la convalida dell'input utente
- Richiedere advange di convalida sul lato client unobstrusive con jQuery in Web Form
- Implementare la convalida delle richieste granulari
- Implementare l'elaborazione in Web Form della pagina asincrona

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Sono necessari gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.

Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: Using Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: Modello di associazione in Web Form ASP.NET](#Exercise1)
2. [Esercizio 2: La convalida dei dati](#Exercise2)
3. [Esercizio 3: Pagina asincrona, l'elaborazione in ASP.NET Web Form](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Esercizio 1: Modello di associazione in Web Form ASP.NET

La nuova versione di Web Form ASP.NET introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza quando si lavora con i dati. In questo esercizio, si verrà Scopri i controlli dati fortemente tipizzati e associazione di modelli.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Attività 1: utilizzo di associazioni dati fortemente tipizzati

In questa attività consente di individuare nuovi fortemente tipizzate binding disponibile in ASP.NET 4.5.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex1-ModelBinding/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **Customers** pagina. Inserire un elenco e il numero di controllo principale e includere un controllo repeater all'interno per l'elenco di ogni cliente. Impostare il nome del ripetitore **customersRepeater** come illustrato nel codice seguente.

    Nelle versioni precedenti di Web Form, quando si usa l'associazione dati per generare il valore di un membro in un oggetto sei data binding, si potrebbe usare un'espressione di associazione dati, insieme a una chiamata al metodo Eval, passando il nome del membro sotto forma di stringa.

    In fase di esecuzione, queste chiamate a Eval usare la reflection per l'oggetto attualmente associato per leggere il valore del membro con il nome specificato e visualizzare il risultato nell'HTML. Questo approccio è molto semplice eseguire l'associazione dei dati rispetto ai dati arbitrari, unshaped.

    Sfortunatamente, si perde molte delle funzionalità straordinaria esperienza nella fase di sviluppo in Visual Studio, tra cui IntelliSense per i nomi dei membri, il supporto per la navigazione (come Vai a definizione) e controllo in fase di compilazione.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Aprire il **Customers.aspx.cs** file.
4. Aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Nel **pagina\_carico** metodo, aggiungere il codice per popolare il controllo repeater con l'elenco dei clienti.

    (Code - Snippet *Web Form Lab - Ex01 - Bind clienti Zdroj dat*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La soluzione Usa EntityFramework insieme CodeFirst per creare e accedere al database. Nel codice seguente, il customersRepeater è associato a una query materializzata che restituisce tutti i clienti dal database.
6. Premere **F5** per eseguire la soluzione e selezionare il **clienti** pagina per visualizzare il controllo repeater in azione. Quando la soluzione Usa CodeFirst, il database verrà creato e popolato nell'istanza locale di SQL Express durante l'esecuzione dell'applicazione.

    ![Elencare i clienti con un controllo repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "elencare i clienti con un controllo repeater")

    *Elencare i clienti con un controllo repeater*

    > [!NOTE]
    > In Visual Studio 2012, IIS Express è il server di sviluppo Web predefinito.
7. Chiudere il browser e tornare a Visual Studio.
8. A questo punto sostituire l'implementazione per l'uso di associazioni fortemente tipizzate. Aprire il **Customers** pagina e usare le nuove **ItemType** attributo nel repeater per impostare il **cliente** tipo come tipo di associazione.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La proprietà tipo di elemento consente di dichiarare il tipo di dati deve essere associato al controllo e consente di usare fortemente tipizzata di associazione all'interno del controllo associato a dati.
9. Sostituire il contenuto con il codice seguente ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Uno svantaggio con gli approcci precedenti è che le chiamate a eval e BIND con associazione tardiva, vale a dire che passi le stringhe per rappresentare i nomi delle proprietà. Ciò significa che non si ottiene Intellisense per i nomi dei membri, il supporto per l'esplorazione del codice (ad esempio, Vai a definizione), né il supporto di controllo della fase di compilazione.

    Se si imposta la proprietà tipo di elemento, due nuove variabili tipizzate da generare nell'ambito delle espressioni di associazione dati: **articoli** e **BindItem**. È possibile usare queste variabili fortemente tipizzate nelle espressioni di associazione dati e ottenere i vantaggi dell'esperienza di sviluppo Visual Studio.

    Il &quot; **:** &quot; utilizzato nell'espressione eseguirà automaticamente la codifica HTML di output in modo da evitare problemi di sicurezza (ad esempio, il cross-site gli attacchi di scripting). Questa notazione era disponibile dalla 4 di .NET per la scrittura di risposta, ma ora è disponibile anche in espressioni di associazione dati.

    > [!NOTE]
    > Membro dell'elemento funziona per l'associazione unidirezionale. Se si desidera eseguire l'associazione bidirezionale Usa la **BindItem** membro.

    ![Supporto di IntelliSense nell'associazione fortemente tipizzate](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "supporto IntelliSense nell'associazione fortemente tipizzate")

    *Supporto di IntelliSense nell'associazione fortemente tipizzate*
10. Premere **F5** per eseguire la soluzione, andare alla pagina di clienti per assicurarsi che le modifiche funzionino come previsto.

    ![I dettagli dei clienti di presentazione](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "listato dettagli cliente")

    *Elenco dettagli cliente*
11. Chiudere il browser e tornare a Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Attività 2 - Introduzione a modello di associazione in Web Form

Nelle versioni precedenti di Web Form ASP.NET, quando si desidera eseguire l'associazione dati bidirezionale, sia il recupero e aggiornamento dei dati, è necessario usare un oggetto origine dati. Potrebbe trattarsi di un'origine dati oggetto, un'origine dati SQL, un'origine dati LINQ e così via. Tuttavia se lo scenario richiesto codice personalizzato per gestire i dati, è necessario usare l'origine dati oggetto e questo introdotti alcuni svantaggi. È necessario gestire le eccezioni durante l'esecuzione di logica di convalida, ad esempio, è necessario per evitare i tipi complessi.

Nella nuova versione di Web Form ASP.NET i controlli con associazione a dati supportano l'associazione di modelli. Ciò significa che è possibile specificare selezionare, aggiornare, inserire ed eliminare i metodi direttamente nel controllo associato a dati da chiamare per la logica da file code-behind o da un'altra classe.

Per altre informazioni, si utilizzerà un controllo GridView per elencare le categorie di prodotto usando le nuove **SelectMethod** attributo. Questo attributo consente di specificare un metodo per recuperare i dati di GridView.

1. Aprire il **Products** pagina e includere una **GridView**. Configurare il controllo GridView, come illustrato di seguito per usare le associazioni fortemente tipizzato e abilitare l'ordinamento e paging.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Usare le nuove **SelectMethod** attributo per configurare il controllo GridView per chiamare un **GetCategories** (metodo) per selezionare i dati.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Aprire il **Products.aspx.cs** code-behind di file e aggiungere quanto segue usando istruzioni.

    (Code - Snippet *Web gli spazi dei nomi di moduli Lab - Ex01 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Aggiungere un membro privato nel **prodotti** classe e assegnare una nuova istanza **ProductsContext**. Questa proprietà archivia il contesto di dati di Entity Framework che consente di connettersi al database.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Creare un **GetCategories** metodo per recuperare l'elenco delle categorie di utilizzo di LINQ. La query includerà il **prodotti** proprietà in modo che il controllo GridView possa mostrare la quantità di prodotti per ogni categoria. Si noti che il metodo restituisce un oggetto IQueryable non elaborato che rappresentano la query viene eseguita in un secondo momento il ciclo di vita di pagina.

    (Code - Snippet *Web Form Lab - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Nelle versioni precedenti di ASP.NET Web Form, l'abilitazione richiesto l'ordinamento e paging tramite la propria logica di repository in un contesto di origine dati dell'oggetto, per scrivere codice personalizzato e la ricezione di tutti i parametri necessari. A questo punto, come i metodi di associazione dati possono restituire IQueryable e ciò rappresenta una query ancora per l'esecuzione, ASP.NET può occuparsi di modifica delle query per aggiungere l'ordinamento corretto e i parametri di paging.
6. Premere **F5** per avviare il debug del sito e passare alla pagina di prodotti. Si dovrebbe vedere che il controllo GridView viene popolato con le categorie restituite dal metodo GetCategories.

    ![Popolamento di un controllo GridView mediante associazione di modelli](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "popolamento di un controllo GridView mediante associazione di modelli")

    *Popolamento di un controllo GridView mediante associazione di modelli*
7. Premere **SHIFT**+**F5** interrompere il debug.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Attività 3: provider di valori di associazione di modelli

Associazione di modelli non solo consente di specificare metodi personalizzati per lavorare con i dati direttamente nel controllo associato a dati, ma consente anche di eseguire il mapping dei dati dalla pagina in parametri di questi metodi. Nel metodo del parametro, è possibile utilizzare gli attributi del provider di valore per specificare l'origine dati del valore. Ad esempio:

- Controlli della pagina
- Valori di stringa di query
- Visualizzare i dati
- Stato sessione
- Cookie
- Dati del modulo registrato
- Stato di visualizzazione
- Provider di valori personalizzati sono supportati anche

Se si usa ASP.NET MVC 4, si noterà che il supporto dell'associazione del modello è simile. In effetti, queste funzionalità sono state eseguite da ASP.NET MVC e spostate nella **System. Web** assembly sia in grado di usarle anche nei Web Form.

In questa attività si aggiornerà il controllo GridView per filtrare i risultati dalla quantità di prodotti per ogni categoria, riceve il parametro di filtro con l'associazione di modelli.

1. Tornare al **Products** pagina.
2. Nella parte superiore di GridView, aggiungere un **Label** e una **ComboBox** per selezionare il numero di prodotti per ogni categoria, come illustrato di seguito.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Aggiungere un **EmptyDataTemplate** a GridView per visualizzare un messaggio quando nessuna categoria con il numero di prodotti selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Aprire il **Products.aspx.cs** code-behind e aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modificare il **GetCategories** metodo per la ricezione di un numero intero **minProductsCount** argomento e filtrare i risultati restituiti. A tale scopo, sostituire il metodo con il codice seguente.

    (Code - Snippet *Web Form Lab - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Il nuovo **[CTRL]** attributo il **minProductsCount** argomento comunica ASP.NET il relativo valore deve essere popolato usando un controllo nella pagina. ASP.NET verrà cercare qualsiasi controllo che corrisponde a quello dell'argomento (minProductsCount) ed eseguire il mapping necessario e conversione da riempire il parametro con il valore del controllo.

    In alternativa, l'attributo fornisce un costruttore di overload che consente di specificare il controllo da cui ottenere il valore.

    > [!NOTE]
    > Uno degli obiettivi delle funzionalità di data binding consiste nel ridurre la quantità di codice che deve essere scritto per l'interazione di pagina. Oltre a provider di valori [CTRL], è possibile usare altri provider di servizi di associazione di modelli nei parametri del metodo. Alcuni di essi sono elencati nella sezione introduttiva di attività.
6. Premere **F5** per avviare il debug del sito e passare alla pagina di prodotti. Selezionare un numero di prodotti nell'elenco a discesa elenco e osservare come il controllo GridView è ora aggiornato.

    ![Filtro GridView con un valore di elenco a discesa](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView con un valore di elenco a discesa elenco filtro")

    *GridView con un valore di elenco a discesa elenco filtro*
7. Terminare il debug.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Attività 4: associazione per il filtro del modello usando

In questa attività si aggiungerà una seconda, figlio GridView per visualizzare i prodotti appartenenti alla categoria selezionata.

1. Aprire il **Products** pagina e aggiornare le categorie GridView per generare automaticamente il pulsante di selezione.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Aggiungere una seconda **GridView** denominate **productsGrid** nella parte inferiore. Impostare il **ItemType** al **WebFormsLab.Model.Product**, il **DataKeyNames** a **ProductId** e il **SelectMethod**  al **GetProducts**. Impostare **AutoGenerateColumns** al **false** e aggiungere le colonne ProductId, ProductName, descrizione e prezzo unitario.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Aprire il **Products.aspx.cs** file code-behind. Implementare il **GetProducts** metodo per ricevere l'ID della categoria dalla categoria GridView e filtrare i prodotti. Associazione del modello verrà impostato il valore del parametro usando la riga selezionata nel **categoriesGrid**. Poiché il nome dell'argomento e il nome di controllo non corrispondono, è necessario specificare il nome del controllo nel provider di valori di controllo.

    (Code - Snippet *Web Form Lab - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Questo approccio rende più semplice per unit test di questi metodi. In un contesto unit test, in cui non è in esecuzione Web Form, l'attributo [CTRL] non eseguirà alcuna azione specifica.
4. Aprire il **Products** pagina e individuare i prodotti GridView. Aggiornare i prodotti GridView per visualizzare un collegamento per la modifica del prodotto selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Aprire il **ProductDetails.aspx** pagina code-behind e sostituire il **SelectProduct** metodo con il codice seguente.

    (Code - Snippet *form Lab - Ex01 - SelectProduct metodo Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Si noti che il **[QueryString]** attributo viene usato per riempire il parametro del metodo da un parametro di productId nella stringa di query.
6. Premere **F5** per avviare il debug del sito e passare alla pagina di prodotti. Selezionare qualsiasi categoria nelle categorie GridView e osservare che i prodotti GridView viene aggiornato.

    ![Che mostra i prodotti della categoria selezionata](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "che mostra i prodotti della categoria selezionata")

    *Che mostra i prodotti della categoria selezionata*
7. Scegliere il **vista** collegamento su un prodotto per aprire la pagina ProductDetails.aspx.

    Si noti che la pagina sta recuperando il prodotto con la proprietà SelectMethod usando il parametro productId dalla stringa di query.

    ![Visualizzare i dettagli del prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "visualizzando i dettagli del prodotto")

    *Visualizzare i dettagli del prodotto*

    > [!NOTE]
    > Nel prossimo esercizio verrà implementata la possibilità di immettere una descrizione HTML.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Attività 5: con modello di associazione per le operazioni di aggiornamento

Nell'attività precedente, si usa l'associazione di modelli principalmente per la selezione di dati, in questa attività si apprenderà come usare l'associazione di modelli nelle operazioni di aggiornamento.

È possibile aggiornare le categorie di GridView per consentire all'utente di aggiornare le categorie.

1. Aprire il **Products** pagina e aggiornare le categorie GridView per generare automaticamente il pulsante di modifica e usare le nuove **UpdateMethod** attributo per specificare un **UpdateCategory**metodo per aggiornare l'elemento selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    L'attributo DataKeyNames in GridView definire quali sono i membri che identificano in modo univoco l'oggetto associato a un modello e di conseguenza, quali sono i parametri che al metodo update almeno deve ricevere.
2. Aprire il **Products.aspx.cs** file code-behind e implementare le **UpdateCategory** (metodo). Il metodo deve ricevere l'ID della categoria per caricare la categoria corrente, popolare i valori da GridView e quindi aggiornare la categoria.

    (Code - Snippet *Web Form Lab - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Il nuovo **TryUpdateModel** metodo nella classe delle pagine è responsabile del popolamento dell'oggetto modello utilizzando i valori dai controlli di pagina. In questo caso, che sostituirà i valori aggiornati rispetto alla riga corrente di GridView in fase di modifica nel **categoria** oggetto.

    > [!NOTE]
    > Esercizio successivo verrà illustrato l'utilizzo del ModelState per la convalida dei dati immessi dall'utente quando si modifica l'oggetto.
3. Esecuzione del sito e passare alla pagina di prodotti. Modificare una categoria. Digitare un nuovo nome e quindi fare clic su **Update** per rendere permanenti le modifiche.

    ![Modifica di categorie](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Modifica categorie")

    *Categorie di modifica*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Esercizio 2: La convalida dei dati

In questo esercizio, si apprenderà sulle nuove funzionalità di convalida dei dati in ASP.NET 4.5. Verificherà le nuove funzionalità di convalida discreta in Web Form. Si userà le annotazioni dei dati nelle classi del modello di applicazione per la convalida dell'input utente, e infine, si apprenderà come attivare o disattivare la convalida della richiesta sui singoli controlli in una pagina.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Attività 1: la convalida discreta

Form dati complessi, inclusi i validator tendono a generare una quantità eccessiva codice JavaScript in una pagina, che può rappresentare circa il 60% del codice. Con la convalida discreta è attivata, il codice HTML avrà un aspetto più pulita e più ordinata.

In questa sezione si abiliterà la convalida discreta in ASP.NET per confrontare il codice HTML generato da entrambe le configurazioni.

1. Aprire **Visual Studio 2012** e aprire il **Begin** soluzione che si trova nel **Source\Ex2 Validation\Begin** cartella della presente esercitazione. In alternativa, è possibile continuare a lavorare sulla tua soluzione esistente nell'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, in Esplora soluzioni, scegliere il **WebFormsLab** project **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Premere **F5** per avviare l'applicazione web. Passare ai clienti di pagina e fare clic il **aggiungere un nuovo cliente** collegamento.
3. Nella pagina del browser e scegliere **Visualizza origine** opzione per aprire il codice HTML generato dall'applicazione.

    ![Che mostra il codice HTML della pagina](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "che mostra il codice HTML della pagina")

    *Che mostra il codice HTML della pagina*
4. Scorrere il codice sorgente della pagina e si noti che ASP.NET è inserito JavaScript codice e dati di validator nella pagina per eseguire le convalide e visualizzare l'elenco errori.

    ![Codice di convalida JavaScript nella pagina CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "codice di convalida JavaScript nella pagina CustomerDetails")

    *Codice di convalida JavaScript nella pagina CustomerDetails*
5. Chiudere il browser e tornare a Visual Studio.
6. A questo punto si abiliterà la convalida discreta. Aprire **Web. config** e individuare **ValidationSettings:UnobtrusiveValidationMode** chiave nel **AppSettings** sezione **.** Impostare il valore della chiave **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > È anche possibile impostare questa proprietà &quot; **pagina\_Load** &quot; evento nel caso si vuole abilitare la convalida discreta solo per alcune pagine.
7. Aprire **CustomerDetails.aspx** , quindi premere **F5** per avviare l'applicazione Web.
8. Premere il tasto F12 per aprire gli strumenti di sviluppo di Internet Explorer. Dopo avere aperto gli strumenti di sviluppo, selezionare la scheda dello script. Selezionare **CustomerDetails.aspx** dal menu e prendere nota che gli script necessari richiesti per l'esecuzione di jQuery nella pagina sono stati caricati nel browser dal sito locale.

    ![Il caricamento di JavaScript di jQuery file direttamente dal server IIS locale](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "il caricamento di JavaScript di jQuery file direttamente dal server IIS locale")

    *Il caricamento di file JavaScript di jQuery direttamente dal server IIS locale*
9. Chiudere il browser per tornare a Visual Studio. Aprire il **Site. master** nuovamente file e individuare le **ScriptManager**. Aggiungere l'attributo **EnableCdn** proprietà con il valore **True**. Questa operazione forzerà jQuery per essere caricato dall'URL online, non dall'URL del sito locale.
10. Aprire **CustomerDetails.aspx** in Visual Studio. Premere il tasto F5 per eseguire il sito. Quando si apre Internet Explorer, premere il tasto F12 per aprire gli strumenti di sviluppo. Selezionare il **Script** scheda e quindi esaminare l'elenco a discesa. Si noti che non vengono caricati i file JavaScript di jQuery non è più dal sito locale, ma piuttosto dalla rete CDN di jQuery online.

    ![Il caricamento di JavaScript di jQuery file dalla rete CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "il caricamento di JavaScript di jQuery file dalla rete CDN")

    *Il caricamento di file JavaScript di jQuery dalla rete CDN*
11. Aprire il codice sorgente di pagina HTML nel browser usando l'opzione di visualizzazione origine. Si noti che abilitando la convalida discreta ASP.NET ha sostituito il codice JavaScript inserito con dati - \*attributi.

    ![Codice di convalida discreta](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "codice Unobtrusive validation")

    *Codice di convalida discreta*

    > [!NOTE]
    > In questo esempio è stato illustrato come un riepilogo con le annotazioni dei dati di convalida è stata semplificata per poche HTML e JavaScript righe. In precedenza, senza la convalida discreta, i controlli di convalida più si aggiungono, più grande il codice di convalida JavaScript aumenteranno.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Attività 2: convalida del modello con le annotazioni dei dati

ASP.NET 4.5 introduce convalida le annotazioni dei dati per i Web Form. Anziché disporre di un controllo di convalida su ogni input, è ora possibile definire vincoli nelle classi di modelli e usarle in tutte le app web. In questa sezione si apprenderà come usare le annotazioni dei dati per la convalida di un form di cliente nuova/modifica.

1. Aprire **CustomerDetail.aspx** pagina. Si noti che il cliente prima di tutto assegnare un nome e nome del secondo nel **EditItemTemplate** e **InsertItemTemplate** sezioni vengono convalidate mediante un RequiredFieldValidator i controlli. Ogni validator è associata a una determinata condizione, pertanto è necessario includere tutti i validator come le condizioni per controllare.
2. Aggiungere annotazioni dei dati per convalidare la classe di modello Customer. Aprire **Customer.cs** classe la **Model** cartella e *decorare* ogni proprietà usando gli attributi di annotazione dei dati.

    (Code - Snippet *Web le annotazioni dei dati di form Lab - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 ha esteso la raccolta di annotazioni dei dati esistenti. Queste sono alcune delle annotazioni dei dati è possibile utilizzare: [CreditCard], [Phone], [EmailAddress], [intervallo], [confrontare], [Url], [FileExtensions], [Required], [Key], [RegularExpression].
    > 
    > Alcuni esempi d'uso:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > È anche possibile definire i propri messaggi di errore all'interno di ogni attributo.
3. Aprire **CustomerDetails.aspx** e rimuovere tutte le RequiredFieldvalidators per i campi nome e cognome nelle sezioni di EditItemTemplate e InsertItemTemplate del controllo FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Uno dei vantaggi dell'uso delle annotazioni dei dati è che la logica di convalida non è duplicata nelle pagine dell'applicazione. Si definiscono una sola volta nel modello e usarlo in tutte le pagine di applicazione che consentono di modificare i dati.
4. Aprire **CustomerDetails.aspx** code-behind e individuare il metodo SaveCustomer. Questo metodo viene chiamato durante l'inserimento di un nuovo cliente e riceve il parametro dei clienti dai valori del controllo FormView. Quando il mapping tra i controlli di pagina e si verifica l'oggetto parametro, ASP.NET verrà eseguita la convalida del modello su tutte le annotazioni di dati di attributi e riempire il dizionario ModelState con gli errori rilevati, se presente.

    Il ModelState solo restituirà true se tutti i campi nel modello sono validi dopo l'esecuzione della convalida.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Aggiungere un **ValidationSummary** controllo alla fine della pagina CustomerDetails per visualizzare l'elenco di errori del modello.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    Il **ShowModelStateErrors** è una nuova proprietà di controllo ValidationSummary controllano che se impostato su **true**, il controllo mostrerà gli errori dal dizionario ModelState. Questi errori provengono dalla convalida le annotazioni dei dati.
6. Premere **F5** per eseguire l'applicazione Web. Completare il modulo con alcuni valori non corretti e fare clic su **salvare** per eseguire la convalida. Si noti che l'errore riepilogo nella parte inferiore.

    ![Convalida con Data Annotations](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "convalida con annotazioni dei dati")

    *Convalida con annotazioni dei dati*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Attività 3: gestione degli errori di Database personalizzata con ModelState

Nella versione precedente di Web Form, la gestione degli errori di database, ad esempio una stringa troppo lunga o una violazione di chiave univoca può comportare la generazione di eccezioni nel codice del repository e quindi la gestione delle eccezioni nel code-behind per visualizzare un errore. È necessario un elevato livello di codice per eseguire un'operazione relativamente semplice.

In Web Form 4.5, l'oggetto ModelState può essere utilizzato per visualizzare gli errori nella pagina, dal modello o dal database, in modo coerente.

In questa attività si aggiungerà codice per gestire le eccezioni di database e visualizzare il messaggio appropriato all'utente usando l'oggetto ModelState correttamente.

1. Mentre l'applicazione è ancora in esecuzione, provare ad aggiornare il nome di una categoria di utilizzo di un valore duplicato.

    ![L'aggiornamento di una categoria con un nome duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "l'aggiornamento di una categoria con un nome duplicato")

    *L'aggiornamento di una categoria con un nome duplicato*

    Si noti che viene generata un'eccezione a causa dell'errore il &quot;univoci&quot; vincolo delle **CategoryName** colonna.

    ![Eccezione per i nomi di categoria duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "eccezione per i nomi di categoria duplicato")

    *Eccezione per i nomi di categoria duplicato*
2. Terminare il debug. Nel **Products.aspx.cs** file code-behind, aggiornare il **UpdateCategory** metodo per gestire le eccezioni generate dal database. Metodo SaveChanges () chiama e si aggiunge un errore per il **ModelState** oggetto.

    Il nuovo **TryUpdateModel** metodo aggiorna l'oggetto categoria recuperato dal database utilizzando i dati del modulo forniti dall'utente.

    (Code - Snippet *Web Form Lab - Ex02 - Handle UpdateCategory errori*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > In teoria, è necessario identificare la causa dell'eccezione DbUpdateException e verificare se la causa principale è la violazione del vincolo di chiave univoca.
3. Aprire **Products** e aggiungere un **ValidationSummary** controllo sotto le categorie di GridView per visualizzare l'elenco di errori del modello.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Esecuzione del sito e passare alla pagina di prodotti. Provare ad aggiornare il nome di una categoria utilizzando un valore duplicato.

    Si noti che è stata gestita l'eccezione e viene visualizzato il messaggio di errore il **ValidationSummary** controllo.

    ![Errore di categoria duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicato categoria errore")

    *Errore di categoria duplicato*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Attività 4: richiedere la convalida in Web Form ASP.NET 4.5

La funzionalità di convalida delle richieste in ASP.NET fornisce un certo livello di protezione predefinita da attacchi di cross-site scripting (XSS). Nelle versioni precedenti di ASP.NET, la convalida della richiesta è stata abilitata per impostazione predefinita e può essere disabilitato solo per un'intera pagina. Con la nuova versione di Web Form ASP.NET è possibile ora disabilitare la convalida delle richieste per un singolo controllo, eseguire la convalida della richiesta lenta o accedere ai dati della richiesta non convalidati (prestare attenzione in caso contrario!).

1. Premere **CTRL+F5** per avviare il sito senza eseguire debug e passare alla pagina di prodotti. Selezionare una categoria e quindi scegliere il **modifica** collegamento su tutti i prodotti.
2. Digitare una descrizione che contiene contenuto potenzialmente dannoso, ad esempio, inclusi i tag HTML. Notare l'eccezione generata a causa la convalida delle richieste.

    ![Modifica di un prodotto con contenuto potenzialmente dannoso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "modifica di un prodotto con contenuto potenzialmente dannoso")

    *Modifica di un prodotto con contenuto potenzialmente dannoso*

    ![Eccezione generata a causa di convalida delle richieste](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "eccezione generata a causa di convalida delle richieste")

    *Eccezione generata a causa di convalida delle richieste*
3. Chiudere la pagina e, in Visual Studio, premere **MAIUSC+F5** per arrestare il debug.
4. Aprire il **ProductDetails.aspx** pagina e individuare le **descrizione** nella casella di testo.
5. Aggiungere il nuovo **ValidateRequestMode** proprietà alla casella di testo e impostarne il valore **disabilitato**.

    Il nuovo **ValidateRequestMode** attributo consente di disabilitare la convalida delle richieste in modo granulare su ogni controllo. Ciò è utile quando si desidera utilizzare un input che possa ricevere il codice HTML, ma vuole mantenere la convalida per il resto della pagina.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Premere **F5** per eseguire l'applicazione web. Riaprire la pagina di prodotto di modifica e completare la descrizione di un prodotto, inclusi i tag HTML. Si noti che è ora possibile aggiungere contenuto HTML per la descrizione.

    ![Richiedere la convalida disabilitata per la descrizione del prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "convalida disabilitata per la descrizione del prodotto della richiesta")

    *Convalida delle richieste disabilitata per la descrizione del prodotto*

    > [!NOTE]
    > In un'applicazione di produzione, è consigliabile purificare il codice HTML immesso dall'utente per assicurarsi che vengano immessi sicuro solo i tag HTML (ad esempio, esistono nessun &lt;script&gt; tag). A tale scopo, è possibile usare [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).
7. Modificare di nuovo il prodotto. Digitare il codice HTML nel campo del nome e fare clic su **salvare**. Si noti che convalida della richiesta è disabilitata solo per il campo della descrizione e il resto dei campi re ancora convalidato rispetto al contenuto potenzialmente pericoloso.

    ![Convalida abilitata nel resto dei campi delle richieste](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "abilitata nel resto dei campi di convalida delle richieste")

    *Convalida delle richieste abilitata nella parte rimanente dei campi*

    Web Form ASP.NET 4.5 include una nuova modalità di convalida richiesta per eseguire la convalida delle richieste in modo differito. Con la modalità di convalida della richiesta impostata su **4.5**, se una parte di codice accede *Request. Form [&quot;key&quot;]*, attivazione solo will convalida richiesta di ASP.NET 4.5 la convalida delle richieste Per questo specifico elemento della raccolta di form.

    Inoltre, ASP.NET 4.5 include ora le routine di codifica core dai v4.0 libreria di Microsoft Anti-XSS. La routine di codifica vengono implementate dalla nuova Anti-XSS *AntiXssEncoder* tipo disponibile nel nuovo **System.Web.Security.AntiXss** dello spazio dei nomi. Con il **encoderType** parametro configurato per l'uso *AntiXssEncoder*, tutto l'output automaticamente la codifica all'interno di ASP.NET usa la nuova routine di codifica.
8. Convalida delle richieste 4.5 ASP.NET supporta anche l'accesso non convalidato di richiedere i dati. ASP.NET 4.5 aggiunge una nuova proprietà di raccolta per il **HttpRequest** oggetto chiamato **Unvalidated**. Quando si passa a **HttpRequest.Unvalidated** potere accedere a tutte le parti comuni dei dati richiesta, tra cui moduli alle stringhe di query, cookie, URL e così via.

    ![Oggetto Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated oggetto")

    *Oggetto Request.Unvalidated*

    > [!NOTE]
    > **Usare invece la proprietà HttpRequest.Unvalidated con cautela.** Assicurarsi di che eseguire con attenzione la convalida personalizzata sui dati richiesta non elaborata per assicurarsi che testo pericoloso non round trip e viene eseguito il rendering al inconsapevole!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Esercizio 3: Pagina asincrona, l'elaborazione in ASP.NET Web Form

In questo esercizio, verrà introdotti alla pagina asincrona nuove funzionalità in Web Form ASP.NET di elaborazione.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Pagina per caricare e visualizzare le immagini di dettagli attività 1: l'aggiornamento del prodotto

In questa attività si aggiornerà la pagina dei dettagli del prodotto per consentire all'utente di specificare un URL dell'immagine per il prodotto e visualizzarlo nella visualizzazione di sola lettura. Si creerà una copia locale dell'immagine specificata, scaricarlo in modo sincrono. Nell'attività successiva, si aggiornerà questa implementazione per il funzionamento in modo asincrono.

1. Aprire **Visual Studio 2012** e caricare il **Begin** soluzione che si trova nella **Source\Ex3 Async\Begin** dalla cartella del lab. In alternativa, è possibile continuare a lavorare sulla tua soluzione esistente dagli esercizi precedenti.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, in Esplora soluzioni, scegliere il **WebFormsLab** del progetto e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **ProductDetails.aspx** pagina origine e aggiungere un campo nell'elemento ItemTemplate del controllo FormView per mostrare l'immagine del prodotto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Aggiungere un campo per specificare l'URL dell'immagine in EditTemplate del controllo FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Aprire il **ProductDetails.aspx.cs** code-behind del file e aggiungere le direttive dello spazio dei nomi seguenti.

    (Code - Snippet *Web Form Lab - Ex03 - degli spazi dei nomi*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Creare un **UpdateProductImage** metodo per archiviare immagini remote in locale **immagini** cartella e aggiornare l'entità product con il nuovo valore di posizione di immagine.

    (Code - Snippet *Web Form Lab - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aggiorna il **UpdateProduct** metodo da chiamare il **UpdateProductImage** (metodo).

    (Code - Snippet *form Lab - Ex03 - UpdateProductImage chiamata Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Eseguire l'applicazione e provare a caricare un'immagine per un prodotto. Ad esempio, è possibile utilizzare l'URL dell'immagine da Office Clip arte seguenti: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![L'impostazione di un'immagine per un prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "l'impostazione di un'immagine per un prodotto")

    *L'impostazione di un'immagine per un prodotto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Pagina dei dettagli attività 2: aggiunta di elaborazione per il prodotto asincrona

In questa attività si aggiornerà la pagina dei dettagli del prodotto per il funzionamento in modo asincrono. Si procederà al miglioramento un'attività a esecuzione prolungata - processo di download dell'immagine - tramite l'elaborazione delle pagine asincrone ASP.NET 4.5.

I metodi asincroni nelle applicazioni web utilizzabile per ottimizzare il modo in cui vengono usati i pool di thread ASP.NET. In ASP.NET si sono richiede un numero limitato di thread nel pool di thread per tenere conto, pertanto, quando tutti i thread sono occupati, ASP.NET Rifiuta nuove richieste, invia i messaggi di errore dell'applicazione e rende disponibile il sito.

Le operazioni che richiedono molto tempo nel proprio sito web sono ottimi candidati per la programmazione asincrona in quanto il thread assegnato che occupano molto tempo. Ciò include le richieste di esecuzione prolungata, le pagine con un numero elevato di elementi diversi e le pagine che richiedono operazioni non in linea, tali query su un database o l'accesso a un server web esterno. Il vantaggio è che se si usano i metodi asincroni per queste operazioni, mentre la pagina viene elaborata, il thread viene liberato e restituito al thread del pool e può essere usato per la partecipazione a una nuova richiesta di pagina. Questo significa che, la pagina comincerà a elaborare in un thread dal pool di thread e l'elaborazione in uno diverso, potrebbe essere completata al termine dell'elaborazione asincrona.

1. Aprire il **ProductDetails.aspx** pagina. Aggiungere il **Async** attributo il **pagina** elemento e impostarlo su **true**. Questo attributo indica ad ASP.NET per implementare l'interfaccia IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Aggiungere un'etichetta nella parte inferiore della pagina per visualizzare i dettagli dei thread di esecuzione della pagina.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Aprire la console di **ProductDetails.aspx.cs** e aggiungere le direttive dello spazio dei nomi seguenti.

    (Code - Snippet *Web gli spazi dei nomi di moduli Lab - Ex03 - 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modificare il **UpdateProductImage** metodo per scaricare l'immagine con un'attività asincrona. Si sostituirà il **WebClient** **DownloadFile** metodo con il **DownloadFileTaskAsync** metodo e includere il **await** (parola chiave).

    (Code - Snippet *Web Form Lab - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    Il RegisterAsyncTask registra una nuova attività asincrona di pagina deve essere eseguito in un altro thread. Riceve un'espressione lambda con l'attività (t) deve essere eseguito. Il **await** parola chiave nel **DownloadFileTaskAsync** metodo converte il resto del metodo in un callback che viene richiamato in modo asincrono dopo il **DownloadFileTaskAsync** completamento del metodo. ASP.NET verrà ripresa l'esecuzione del metodo da gestire automaticamente tutti i HTTP richiesta i valori originali. Il nuovo modello di programmazione asincrono in .NET 4.5 consente di scrivere codice asincrono che è molto simile al codice sincrono e consentire al compilatore di gestire le complicazioni del codice di continuazione o funzioni di callback.
    > [!NOTE]
    > RegisterAsyncTask e PageAsyncTask erano disponibili già dopo .NET 2.0. La parola chiave await è nuova dal modello di programmazione asincrono .NET 4.5 e può essere usata con i nuovi metodi TaskAsync dall'oggetto .NET WebClient.
5. Aggiungere codice per visualizzare i thread in cui il codice iniziata e terminata l'esecuzione. A tale scopo, aggiornare il **UpdateProductImage** metodo con il codice seguente.

    (Code - Snippet *thread di Web Form Lab - Ex03 - Show*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Aprire il sito web **Web. config** file. Aggiungere la variabile dell'impostazione App seguente.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Premere **F5** per eseguire l'applicazione e caricare un'immagine per il prodotto. Si noti che l'ID di thread in cui il codice di inizio e fine potrebbe essere diverso. Infatti, eseguire le attività asincrone in un thread separato dal pool di thread ASP.NET. Al completamento dell'attività, ASP.NET inserisce l'attività nuovamente nella coda e assegna uno qualsiasi dei thread disponibili.

    ![Scaricare un'immagine in modo asincrono](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "scarica un'immagine in modo asincrono")

    *Scaricare un'immagine in modo asincrono*

> [!NOTE]
> Inoltre, è possibile distribuire questa applicazione a Azure segue [appendice b: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questa esercitazione pratica i concetti seguenti sono stati risolti e illustrati:

- Usare le espressioni di associazione dati fortemente tipizzati
- Le nuove funzionalità di associazione del modello di Web Form
- Usare i provider di valori per il mapping di dati della pagina per i metodi code-behind
- Usare le annotazioni dei dati per la convalida dell'input utente
- Richiedere advange di convalida sul lato client unobstrusive con jQuery in Web Form
- Implementare la convalida delle richieste granulari
- Implementare l'elaborazione in Web Form della pagina asincrona

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

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
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Visual Studio Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

In questa appendice spiega come creare un nuovo sito web dal portale di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Attività 1: creazione di un nuovo sito Web dal portale di Azure

1. Andare alla [portale di gestione Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico. È possibile effettuare l'iscrizione [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale di Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "accedere al portale di Azure")

    *Accedere al portale*
2. Fare clic su **New** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **Compute** | **sito Web**. Quindi selezionare **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Azure è l'host di un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completata in Azure all'esterno del portale. Non include i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "creando un nuovo sito Web mediante Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna. Verificare che il nuovo sito Web sia in funzione.

    ![Passare al nuovo sito web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "passare al nuovo sito web")

    *Passare al nuovo sito web*

    ![Sito Web in esecuzione](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione sito web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "aprire le pagine di gestione sito web")

    *Aprire le pagine di gestione sito Web*
7. Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in Azure.

    ![Download del sito web di profilo di pubblicazione](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "scaricando il sito web di profilo di pubblicazione")

    *Profilo di pubblicazione scaricato il sito Web*
8. Scaricare il file di profilo di pubblicazione in una posizione nota. In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web in Azure da Visual Studio.

    ![Salvare il file di profilo di pubblicazione](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "salvataggio del profilo di pubblicazione")

    *Salvare il file di profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server. Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Azure all'indirizzo **database Sql** | **server** | **Dashboard del Server**. Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi. Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard di Server di Database SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Dashboard di Server di Database SQL")

    *Dashboard di Server di Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) pulsante.

    ![Aggiunta indirizzo IP del Client](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.

    ![Pubblicazione dell'applicazione](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "l'importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Dopo aver completata la convalida fare clic su **successivo**.

    > [!NOTE]
    > La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.

    ![La convalida connessione](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).

    ![Configurazione della distribuzione Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.
   - Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.
   - Nelle **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome del database.

     ![Configurazione di stringa di connessione di destinazione](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si userà per connettersi al Database SQL di Azure viene visualizzata all'interno di connessione predefinita nella casella di testo. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **Preview** pagina, fare clic su **Publish**.

    ![Pubblicazione dell'applicazione web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "pubblicazione dell'applicazione web")

    *Pubblicazione dell'applicazione web*
9. Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*
