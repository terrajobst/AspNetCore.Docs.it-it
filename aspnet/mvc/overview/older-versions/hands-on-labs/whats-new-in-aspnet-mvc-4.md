---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: What ' s New in ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 è un framework per compilare applicazioni scalabili, basati su standard web usando le potenzialità di ASP.NET e modelli di progettazione consolidati e...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9d5a51a5887ecbbc96fce1416b88aa849bc3674e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912722"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>What ' s New in ASP.NET MVC 4

da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 è un framework per compilare applicazioni scalabili, basati su standard web usando le potenzialità di ASP.NET e .NET framework e modelli di progettazione consolidati. Questa nuova, quarta versione del framework è incentrato su come rendere più semplice lo sviluppo di applicazioni web per dispositivi mobili.

Per iniziare, quando si crea un nuovo progetto ASP.NET MVC 4 è ora disponibile un modello di progetto di applicazione per dispositivi mobili che è possibile usare per compilare un'app autonoma in modo specifico per i dispositivi mobili. Inoltre, ASP.NET MVC 4 si integra con jQuery Mobile tramite un pacchetto NuGet jQuery.Mobile.MVC. jQuery Mobile è un framework basato su HTML5 per lo sviluppo di App web che sono compatibili con tutte le piattaforme dei dispositivi mobili più diffusi, tra cui Windows Phone, iPhone, Android e così via. Tuttavia, se è necessario specializzazione, ASP.NET MVC 4 consente inoltre di fornire viste diverse per diversi dispositivi e fornire le ottimizzazioni specifiche del dispositivo.

In questo laboratorio pratico, iniziare con ASP.NET MVC 4 &quot;applicazione Internet&quot; modello di progetto per creare un'applicazione Raccolta foto. Si procederà al miglioramento progressivo l'app usando nuove funzionalità di ASP.NET MVC 4 e jQuery Mobile per renderlo compatibile con diversi dispositivi mobili e i browser desktop. Si apprenderà anche sul nuovo recipe di codice per la generazione di codice e come ASP.NET MVC 4 rende più semplice per la scrittura di metodi di azione asincroni tramite il supporto di attività&lt;ActionResult&gt; tipi restituiti.

> [!NOTE]
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit). Il progetto specifico per questo lab è disponibile all'indirizzo [What ' s New in Web Form in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Sfruttare i vantaggi dei miglioramenti apportati a di ASP.NET MVC progetto modelli, che comprende il nuovo modello di progetto di applicazione per dispositivi mobili
- Utilizzare l'attributo viewport HTML5 e query sui supporti CSS per migliorare la visualizzazione su dispositivi mobili
- Usare jQuery Mobile per i miglioramenti progressivi e la creazione di web touchscreen ottimizzata dell'interfaccia utente
- Creazione di visualizzazioni specifiche per dispositivi mobili
- Utilizzare il componente di cambio di visualizzazione per passare tra le visualizzazioni per dispositivi mobili e desktop nell'applicazione
- Creare controller asincrono usando il supporto di attività

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Sono necessari gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice B](#AppendixB) per istruzioni su come installarlo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluso nell'installazione di Microsoft Visual Studio 2012)
- Emulatore Windows Phone (incluso nel [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Facoltativo: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con **Electric Plum iPhone simulatore** estensione (solo per i 3 esercizio consente di visualizzare l'applicazione web con un simulatore di iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

In tutto il documento di laboratorio, verrà invitati a inserire blocchi di codice. Per praticità, la maggior parte di tale codice viene fornito come Visual Studio frammenti di codice che è possibile utilizzare da Visual Studio per evitare di dover aggiungerla manualmente.

Per installare i frammenti di codice:

1. Aprire una finestra di Windows Explorer e passare a del lab **Source\Setup** cartella.
2. Fare doppio clic il **Setup. cmd** file in questa cartella in cui installare i frammenti di codice di Visual Studio.

Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice a: Using Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Nuovi modelli di progetto ASP.NET MVC 4](#Exercise1)
2. [Creazione dell'applicazione Web di raccolta foto](#Exercise2)
3. [Aggiunta del supporto per i dispositivi mobili](#Exercise3)
4. [Utilizzo di controller asincroni](#Exercise4)

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Esercizio 1: Nuovi modelli di progetto ASP.NET MVC 4

In questo esercizio si esploreranno i miglioramenti nei modelli di progetto ASP.NET MVC 4. Oltre al modello di applicazione Internet, già presente in MVC 3, questa versione include ora un modello separato per le applicazioni per dispositivi mobili. In primo luogo, esaminerà alcune funzionalità pertinenti della ognuno dei modelli. Quindi, verranno usati per il rendering della pagina in modo corretto su piattaforme diverse utilizzando l'approccio giusto.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Attività 1: esplorare il modello di applicazione Internet

1. Aprire **Visual Studio**.
2. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo, seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere **applicazione Web ASP.NET MVC 4.** Denominare il progetto **PhotoGallery**, selezionare una località (o lasciare il valore predefinito) e fare clic su **OK**.

    > [!NOTE]
    > Successivamente si personalizzerà la soluzione PhotoGallery ASP.NET MVC 4 a questo punto, si sta creando.

    ![Crea un nuovo progetto](whats-new-in-aspnet-mvc-4/_static/image1.png "creando un nuovo progetto")

    *Crea un nuovo progetto*
3. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona la **applicazione Internet** modello di progetto e fare clic su **OK**. Verificare che è stato selezionato Razor come motore di visualizzazione.

    ![Crea una nuova applicazione Internet ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "crea una nuova applicazione Internet ASP.NET MVC 4")

    *Crea una nuova applicazione Internet ASP.NET MVC 4*

    > [!NOTE]
    > Sintassi Razor è stato introdotto in ASP.NET MVC 3. L'obiettivo consiste nel ridurre al minimo il numero di caratteri e sequenze di tasti richieste in un file, abilitando un veloce e fluido codifica del flusso di lavoro. Razor Usa esistente c# / VB (o altro) competenze linguistiche e offre una sintassi di markup di modello che consente un'incredibile HTML costruzione del flusso di lavoro.
4. Premere **F5** per eseguire la soluzione e visualizzare i modelli rinnovati. È possibile estrarre le funzionalità seguenti:

    **Modelli di stile moderno**

    I modelli sono stati rinnovati, che fornisce altri stili dall'aspetto moderno.

    ![I modelli ASP.NET MVC 4 modificato nello stile](whats-new-in-aspnet-mvc-4/_static/image3.png "modificato nello stile di modelli MVC 4")

    *Modelli ASP.NET MVC 4 modificato nello stile*

    ![Nuova pagina di contatto](whats-new-in-aspnet-mvc-4/_static/image4.png "pagina nuovo contatto")

    *Nuova pagina di contatto*

    **Rendering adattivo**

    Consulta il ridimensionamento della finestra del browser e notare come il layout di pagina si adatta dinamicamente per le nuove dimensioni della finestra. Questi modelli utilizzano la tecnica di rendering adattivo per eseguire il rendering correttamente in piattaforme mobili e desktop senza eseguire alcuna personalizzazione.

    ![Modello di progetto ASP.NET MVC 4 in dimensioni di browser differenti](whats-new-in-aspnet-mvc-4/_static/image5.png "modello di progetto ASP.NET MVC 4 in dimensioni diversa del browser")

    *Modello di progetto ASP.NET MVC 4 in dimensioni diversa del browser*

    **Interfaccia utente più completa con JavaScript**

    Un altro miglioramento per i modelli di progetto predefinito è l'uso di JavaScript per fornire un JavaScript più interattiva. I collegamenti di accesso e registrazione usati nel modello di esemplificare da utilizzare il componente aggiuntivo jQuery convalide per convalidare i campi di input dal lato client.

    ![Convalida di jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Convalida di jQuery*

    > [!NOTE]
    > Si noti che i due log nelle sezioni nella prima sezione è possibile accedere usando un account registrato dal sito e nella seconda sezione che è possibile accedere altenativelly utilizzando un altro servizio di autenticazione, ad esempio google (disabilitato per impostazione predefinita).
5. Chiudere il browser per arrestare il debugger, tornare a Visual Studio.
6. Aprire il file **AuthConfig.cs** sotto il **App\_avviare** cartella.
7. Rimuovere il commento dall'ultima riga per registrare il client di Google per *OAuth* l'autenticazione.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Si noti che è possibile abilitare facilmente l'autenticazione usando qualsiasi servizio OAuth o OpenID, ad esempio Facebook, Twitter, Microsoft, e così via.
8. Premere **F5** per eseguire la soluzione e passare alla pagina di accesso.
9. Selezionare **Google** servizio per l'accesso.

    ![Selezione del log nel servizio](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Selezione del log nel servizio*
10. Accedere usando l'account Google.
11. Consentire al sito (localhost) per recuperare informazioni dall'account di Google.
12. Infine, è necessario registrarsi nel sito per associare l'account Google.

   ![Associare il proprio account Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Associare l'account Google*
13. Chiudere il browser per arrestare il debugger, tornare a Visual Studio.
14. È ora possibile esplorare la soluzione di estrarre alcune altre nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.

   ![Il modello di progetto applicazione Internet ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "il modello di progetto applicazione Internet ASP.NET MVC 4")

   *Il modello di progetto applicazione Internet ASP.NET MVC 4*

   - **HTML 5 Markup**

       Sfogliare le viste del modello per individuare il nuovo tag di tema.

       ![Nuovo modello, utilizzando il markup About.cshtml Razor e HTML5. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Nuovo modello, utilizzando il markup About.cshtml Razor e HTML5.")

       *Nuovo modello, utilizzando il markup di HTML5 e Razor (cshtml).*
   - **Librerie JavaScript aggiornate**

       Il modello predefinito di ASP.NET MVC 4 include ora Knockout. js, un framework MVVM di JavaScript che consente di creare avanzate e le applicazioni web altamente reattive con JavaScript e HTML. Ad esempio in MVC 3, jQuery e librerie dell'interfaccia utente di jQuery sono inoltre inclusi in ASP.NET MVC 4.

     > [!NOTE]
     > È possibile ottenere altre informazioni sulla libreria Knockout. js in questo collegamento: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Inoltre, sono disponibili informazioni jQuery UI e jQuery nelle [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Attività 2: esplorare il modello di applicazione per dispositivi mobili

ASP.NET MVC 4 facilita lo sviluppo di siti Web per dispositivi mobili e browser tablet. Questo modello ha la stessa struttura dell'applicazione come il modello di applicazione Internet (si noti che il codice del controller è praticamente identico), ma è stato modificato lo stile per il rendering correttamente in dispositivi mobili basati su touch.

1. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo, seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere il **applicazione Web ASP.NET MVC 4.** Denominare il progetto **PhotoGallery.Mobile**, selezionare una località (o lasciare l'impostazione predefinita), selezionare &quot;aggiungere alla soluzione&quot; e fare clic su **OK**.
2. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona la **dell'applicazione per dispositivi mobili** modello di progetto e fare clic su **OK**. Verificare che è stato selezionato Razor come motore di visualizzazione.

    ![Crea una nuova applicazione per dispositivi mobili ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "crea una nuova applicazione per dispositivi mobili ASP.NET MVC 4")

    *Crea una nuova applicazione per dispositivi mobili ASP.NET MVC 4*
3. A questo punto è in grado di esplorare la soluzione e Scopri alcune delle nuove funzionalità introdotte dal modello di soluzione ASP.NET MVC 4 per dispositivi mobili:

    - **jQuery Mobile libreria**

        Il modello di progetto di applicazione per dispositivi mobili include la libreria jQuery Mobile, che è una libreria open source per la compatibilità di browser per dispositivi mobili. jQuery Mobile applica miglioramento progressivo a browser per dispositivi mobili che supportano JavaScript e CSS. Miglioramento progressivo consente a tutti i browser visualizzare il contenuto di base di una pagina web, mentre Abilita solamente i browser più potenti visualizzare il contenuto avanzato. I file CSS e JavaScript, inclusi in jQuery Mobile stile, consentono di browser per dispositivi mobili per adattare il contenuto nella schermata senza apportare modifiche nel markup della pagina.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *libreria mobile jQuery inclusa nel modello*
    - **Markup basati su HTML5**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Modello di applicazione per dispositivi mobili tramite markup HTML5, (cshtml e index. cshtml)*
4. Premere **F5** per eseguire la soluzione.
5. Aprire il **Windows Phone 7 emulatore**.
6. Nella schermata start phone, aprire Internet Explorer. Estrarre l'URL in cui avviata l'applicazione desktop e passare a tale URL dal telefono (ad esempio `http://localhost:[PortNumber]/`).
7. A questo punto è in grado di immettere la pagina di accesso o estrarre le informazioni sulla pagina. Si noti che lo stile del sito Web è basato sulla nuova app Metro per dispositivi mobili. Il modello di progetto ASP.NET MVC 4 viene visualizzato correttamente sui dispositivi mobili, assicurandosi che tutti gli elementi della pagina siano visibili e abilitati. Si noti che i collegamenti nell'intestazione sono sufficientemente grandi per essere selezionato o toccato.

    ![Progetti di modelli delle pagine in un dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image14.png "progetto modelli delle pagine in un dispositivo mobile")

    *Pagine modello di progetto in un dispositivo mobile*
8. Il nuovo modello Usa anche il **Viewport metatag**. Browser per dispositivi mobili più definire una larghezza per una finestra del browser virtuale oppure &quot;viewport&quot;, che è maggiore della larghezza effettiva del dispositivo mobile. In questo modo il browser per dispositivi mobili visualizzare l'intera pagina web all'interno di schermo virtuale. Il **Viewport metatag** consente agli sviluppatori web impostare la larghezza, altezza e la scala dell'area del browser per dispositivi mobili **.** Il modello di ASP.NET MVC 4 per le applicazioni per dispositivi mobili imposta il viewport per la larghezza del dispositivo (&quot;larghezza = larghezza dispositivo&quot;) nel modello di layout (*Views\Shared\_layout. cshtml*), in modo che tutti i pagine verranno impostato il viewport per la larghezza dello schermo di dispositivi. Si noti che il tag meta Viewport non cambierà la visualizzazione di esplorazione predefinita.
9. Open  **\_layout. cshtml**, all'interno di **viste | Condiviso** cartella e commento per il tag meta Viewport. Eseguire l'applicazione, se non è già aperto e Scopri le differenze.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Il sito dopo l'aggiunta di commenti il tag meta viewport](whats-new-in-aspnet-mvc-4/_static/image15.png "il sito dopo l'aggiunta di commenti il tag meta viewport")

*Il sito dopo l'aggiunta di commenti il tag meta viewport*
10. In Visual Studio, premere **SHIFT** + **F5** per arrestare il debug dell'applicazione.
11. Rimuovere il commento del tag meta viewport.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Attività 3: Rendering adattivo utilizzo

In questa attività si apprenderà a un altro metodo per eseguire il rendering di una pagina Web correttamente nei dispositivi mobili e Web browser allo stesso tempo senza eseguire alcuna personalizzazione. Tag meta Viewport è già stato utilizzato con uno scopo simile. A questo punto si soddisfano un altro metodo potente: *rendering adattivo*.

Rendering adattivo è una tecnica che utilizza **query sui supporti di CSS3** per personalizzare lo stile applicato a una pagina. Query sui supporti definire condizioni all'interno di un foglio di stile, il raggruppamento di stili CSS in una determinata condizione. Solo quando la condizione è true, viene applicato lo stile per gli oggetti dichiarati.

La flessibilità offerta dalla tecnica di rendering adattivo consente qualsiasi personalizzazione per la visualizzazione del sito in diversi dispositivi. È possibile definire tutti gli stili desiderati in un foglio di stile singolo senza scrivere codice per la logica per scegliere lo stile. Pertanto, è un modo molto chiaro per adeguare gli stili di pagina, in quanto riduce la quantità di codice duplicato e per la logica per ragioni di rendering. D'altra parte, della larghezza di banda aumenterebbe, come le dimensioni dei file CSS può avere dimensioni leggermente.

Tramite la tecnica di rendering adattivo, il sito sarà **visualizzati in modo corretto, indipendentemente dal browser.** Tuttavia, è necessario considerare se carico la larghezza di banda aggiuntiva rappresenta un problema.

> [!NOTE]
> Il formato di base di una query di supporto è: @media \[ambito: tutti i | palmari | stampa | proiezione | schermata\] ([proprietà: valore] e... [proprietà: valore])


Esempi di query sui supporti: &gt;  <strong>@media tutte e (max-width: 1000px) e (min-width: 700px) {}:</strong> per tutte le risoluzioni tra 700px e 1000px.

> <strong>@media schermata e (min-width: 400px) e (max-width: 700px) {...}:</strong> solo per le schermate. La risoluzione deve essere compreso tra 400 e 700px.
> 
> <strong>@media palmari e (min-width: 20em), dello schermo e (min-width: 20em) {...}:</strong> per palmari (per dispositivi mobili e dispositivi) e le schermate. La larghezza minima deve essere maggiore di 20em.
> 
> È possibile trovare altre informazioni su questo sul [W3C sito](http://www.w3.org/TR/css3-mediaqueries/).


Si esaminerà ora come funziona il rendering, migliorando la leggibilità di ASP.NET MVC 4 predefiniti di modello di sito Web.

1. Aprire il **PhotoGallery.sln** soluzione è stato creato al passaggio 1 e selezionare il **PhotoGallery** progetto. Premere **F5** per eseguire la soluzione.
2. Ridimensionare la larghezza del browser, l'impostazione di windows per la metà o meno di un quarto delle dimensioni originali. Si noti che ciò che accade con gli elementi nell'intestazione: alcuni elementi non verranno visualizzati nell'area visibile dell'intestazione.
3. Aprire <strong>CSS</strong> file da Esplora soluzioni di Visual Studio, che si trova <strong>contenuto</strong> cartella del progetto. Premere <strong>CTRL + F</strong> per aprire ricerca integrata in Visual Studio e scrivere <strong>@media</strong> individuare il <strong>query supporti CSS</strong>.

    La condizione di query di supporto definita in questo modello funziona in questo modo: quando sono di dimensioni della finestra del browser sotto **850 px**, le regole CSS applicate sono quelle definite all'interno del blocco supporti.

    ![Individuare la query di supporto](whats-new-in-aspnet-mvc-4/_static/image16.png "individuare la query supporti")

    *Individuare la query supporti*
4. Sostituire il valore dell'attributo larghezza massima impostato di 850 px con **10px**, in modo da disabilitare il rendering e premere **CTRL + S** per salvare le modifiche. Tornare al browser e premere **CTRL + F5** per aggiornare la pagina con le modifiche apportate. Notare le differenze in entrambe le pagine quando si modifica la larghezza della finestra.

    ![A sinistra, consiste nell'applicare la pagina il @media stile, destra, lo stile viene omesso](whats-new-in-aspnet-mvc-4/_static/image17.png "a sinistra, consiste nell'applicare la pagina il @media stile, destra, lo stile viene omessa")

    <em>A sinistra, consiste nell'applicare la pagina di @media stile, destra, lo stile viene omessa</em>

    A questo punto, è possibile controllare cosa accade nei dispositivi mobili:

    ![A sinistra, consiste nell'applicare la pagina il @media stile, destra, lo stile viene omesso](whats-new-in-aspnet-mvc-4/_static/image18.png "a sinistra, consiste nell'applicare la pagina il @media stile, destra, lo stile viene omessa")

    <em>A sinistra, consiste nell'applicare la pagina di @media stile, destra, lo stile viene omessa</em>

    Sebbene si noterà che le modifiche quando viene eseguito il rendering della pagina in un Web browser non sono molto importanti quando si usa un dispositivo mobile diventano più evidenti le differenze. Sul lato sinistro dell'immagine, possiamo vedere che lo stile personalizzato migliorato la leggibilità.

    Rendering adattivo è utilizzabile in più scenari, rendendo più semplice applicare lo stile a un sito Web e la risoluzione dei problemi comuni di stile di un approccio chiaro condizionale.

    Il tag meta Viewport e query sui supporti CSS non sono specifici di ASP.NET MVC 4, pertanto è possibile sfruttare queste funzionalità in qualsiasi applicazione web.
5. In Visual Studio, premere **SHIFT** + **F5** per arrestare il debug dell'applicazione.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Esercizio 2: Creazione dell'applicazione Web di raccolta foto

In questo esercizio, verrà utilizzata in un'applicazione Raccolta foto per visualizzare le foto. Inizierà con il modello di progetto ASP.NET MVC 4 e quindi si aggiungerà una funzionalità per recuperare le foto da un servizio e li visualizzano nella home page.

Nel seguente esercizio, si aggiornerà questa soluzione per migliorare il modo in cui che viene visualizzato nei dispositivi mobili.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Attività 1: creazione di un servizio di simulazione foto

In questa attività si creerà una simulazione del servizio per recuperare il contenuto che verrà visualizzato nella raccolta di foto. A tale scopo, si aggiungerà un nuovo controller che restituirà semplicemente un file JSON con i dati di ciascuna foto.

1. Aprire **Visual Studio** se non è già aperto.
2. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo, seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere **applicazione Web ASP.NET MVC 4.** Denominare il progetto **PhotoGallery**, selezionare una località (o lasciare il valore predefinito) e fare clic su **OK**. In alternativa, è possibile continuare a lavorare da esistente ASP.NET MVC 4 **applicazione Internet** soluzione dal **esercizio 1** e ignorare il passaggio successivo.
3. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona la **applicazione Internet** modello di progetto e fare clic su **OK**. Assicurarsi che sia selezionato come il motore di visualizzazione Razor.
4. Nel **Esplora soluzioni**, fare doppio clic il **App\_dati** cartella del progetto e selezionare **Add | Elemento esistente**. Individuare il **Source\Assets\App\_dati** cartella di questa esercitazione e aggiungere il **Photos.json** file.
5. Creare un nuovo controller con il nome **PhotoController**. A tale scopo, fare clic sul **controller** cartella, passa alla **Add** e selezionare **Controller.** Completare il nome del controller, lasciare il **controller MVC vuoto** modello, quindi scegliere **Add**.

    ![Aggiunta di PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "aggiungendo il PhotoController")

    *Aggiunta di PhotoController*
6. Sostituire il **indice** metodo con il codice seguente **Gallery** azione e restituire il contenuto dal file JSON è stato aggiunto recentemente al progetto.

    (Code - Snippet *azione di ASP.NET MVC 4 Lab - Ex02 - raccolta*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Premere **F5** per eseguire la soluzione e quindi passare all'URL seguente per testare il servizio di foto fittizie: `http://localhost:[port]/photo/gallery` (il valore [porta] dipende da porta corrente in cui è stata avviata l'applicazione). La richiesta a questo URL deve recuperare il contenuto di **Photos.json** file.

    ![Test del servizio di foto fittizie](whats-new-in-aspnet-mvc-4/_static/image20.png "test del servizio di foto fittizia")

    *Test del servizio di foto fittizia*

In un'implementazione reale è possibile usare [API Web ASP.NET](../../../../web-api/index.md) per implementare il servizio di raccolta foto. API Web ASP.NET è un framework che rende più semplice compilare servizi HTTP che soddisfano una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è la piattaforma ideale per compilare applicazioni RESTful in .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Attività 2 - visualizzazione di raccolta foto

In questa attività si aggiornerà la Home page per visualizzare la raccolta di foto usando il servizio fittizia creata nella prima attività di questo esercizio. Si aggiungerà i file di modello e aggiornare le visualizzazioni di raccolta.

1. In Visual Studio, premere **SHIFT** + **F5** per arrestare il debug dell'applicazione.
2. Creare il **foto** classe la **modelli** cartella. A tale scopo, fare clic sui **modelli** cartella e selezionare **Add** e fare clic su **classe**. Quindi, impostare il nome su **Photo.cs** e fare clic su **Add**.
3. Aggiungere i membri seguenti per il **foto** classe.

    (Code - Snippet *modello di ASP.NET MVC 4 Lab - Ex02 - foto*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Aprire il file **HomeController.cs** dalla cartella **Controller**.
5. Aggiungere le istruzioni using seguenti.

    (Code - Snippet *ASP.NET MVC 4 Lab - Ex02 - HomeController using*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Aggiornamento di **indice** azione da utilizzare **HttpClient** per recuperare i dati di raccolta e quindi usare il **JavaScriptSerializer** a deserializzarlo al modello di visualizzazione.

    (Code - Snippet *ASP.NET MVC 4 Lab - Ex02 - indice azione*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Aprire il **index. cshtml** file che si trova sotto il **Views\Home** cartella e sostituire tutto il contenuto con il codice seguente.

    Il codice scorre in ciclo tutte le foto recuperate dal servizio e li visualizza in un elenco non ordinato.

    (Code - Snippet *elenco di foto di ASP.NET MVC 4 Lab - Ex02 -*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. Nel **Esplora soluzioni**, fare doppio clic il **contenuto** cartella del progetto e selezionare **Add | Elemento esistente**. Individuare il **Source\Assets\Content** cartella di questa esercitazione e aggiungere il **CSS** file. Devi confermare relativa sostituzione. Se si dispone di **CSS** il file è aperto, sarà necessario confermare che si vuole per ricaricare il file anche.
9. Aprire Esplora File e copiare l'intera **foto** cartella si trova sotto il **Source\Assets** cartella di questo laboratorio nella cartella radice del progetto in Esplora soluzioni.
10. Eseguire l'applicazione. Si dovrebbe visualizzare le foto nella raccolta la home page.

    ![Raccolta foto](whats-new-in-aspnet-mvc-4/_static/image21.png "raccolta foto")

    *Raccolta foto*
11. In Visual Studio, premere **SHIFT** + **F5** per arrestare il debug dell'applicazione.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Esercizio 3: Aggiunta del supporto per i dispositivi mobili

Uno degli aggiornamenti della chiave in ASP.NET MVC 4 è il supporto per lo sviluppo per dispositivi mobili. In questo esercizio verranno esplorati nuove funzionalità di ASP.NET MVC 4 per applicazioni per dispositivi mobili estendendo la soluzione PhotoGallery creata nell'esercizio precedente.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Attività 1: installazione jQuery Mobile in un'applicazione ASP.NET MVC 4

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex3-MobileSupport/inizio/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **Console di gestione pacchetti** facendo clic la **strumenti** > **Gestione pacchetti NuGet** > **Console di gestione pacchetti**  l'opzione di menu.

    ![Apertura della Console di gestione pacchetti NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "aprendo la Console di gestione pacchetti NuGet")

    *Aprire la Console di gestione pacchetti NuGet*
3. Nella Console di gestione pacchetti eseguire il comando seguente per installare il **jQuery.Mobile.MVC** pacchetto.

    jQuery Mobile è una libreria open source per la creazione di interfaccia utente web ottimizzato per il tocco. Il pacchetto NuGet jQuery.Mobile.MVC include gli helper per l'uso di jQuery Mobile con un'applicazione ASP.NET MVC 4.

    > [!NOTE]
    > Eseguendo il comando seguente, si scaricherà la libreria jQuery.Mobile.MVC da Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Questo comando Installa jQuery Mobile e alcuni file di supporto, inclusi i seguenti:

    - **Views/Shared/\_cshtml**: è un jQuery Mobile layout ottimizzato per uno schermo piccolo. Quando il sito Web riceve una richiesta da un browser per dispositivi mobili, che sostituirà il layout originale (\_layout. cshtml) con questo.
    - Un componente di cambio di visualizzazione: costituito il **Views/Shared/\_ViewSwitcher.cshtml** visualizzazione parziale e il **ViewSwitcherController.cs** controller. Questo componente visualizzerà un collegamento nel browser per dispositivi mobili per consentire agli utenti di passare alla versione desktop della pagina.  
        ![Progetto di raccolta foto con supporto per dispositivi mobili](whats-new-in-aspnet-mvc-4/_static/image23.png "progetto raccolta foto con supporto per dispositivi mobili")

        *Progetto di raccolta foto con supporto per dispositivi mobili*
4. Registrare i dispositivi mobili bundle. A tale scopo, aprire il **Global.asax.cs** file e aggiungere la riga seguente.

    (Code - Snippet *bundle per dispositivi mobili ASP.NET MVC 4 Lab - Ex03 - Register*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Eseguire l'applicazione usando un web browser desktop.
6. Aprire il **emulatore Windows Phone 7** che si trova **Menu Start | Tutti i programmi | Windows Phone SDK 7.1 | Emulatore Windows Phone.**
7. Nella schermata start phone, aprire Internet Explorer. Estrarre l'URL in cui avviare l'applicazione e passare a tale URL con il browser del telefono (ad esempio `http://localhost:[PortNumber]/`).

    Si noterà che l'applicazione avrà un aspetto diverso nell'emulatore Windows Phone, come il jQuery.Mobile.MVC ha creato nuovi asset nel progetto che mostrano viste ottimizzate per i dispositivi mobili.

    Si noti che il messaggio nella parte superiore del telefono, che mostra il collegamento che consente di passare alla visualizzazione Desktop. Inoltre, il  **\_cshtml** layout in cui è stato creato dal pacchetto è stato installato, incluso un altro layout nell'applicazione.

    > [!NOTE]
    > Finora, non vi è alcun collegamento per tornare alla visualizzazione per dispositivi mobili. Verrà incluso nelle versioni successive.

    ![Visualizzazione per dispositivi mobili della pagina Home page raccolta foto](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile visualizzazione della pagina Home page raccolta foto")

    *Visualizzazione per dispositivi mobili della pagina Home page raccolta foto*
8. In Visual Studio, premere **SHIFT** + **F5** per arrestare il debug dell'applicazione.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Attività 2: creazione di visualizzazioni per dispositivi mobili

In questa attività si creerà una versione mobile della visualizzazione index con contenuto adattato per una migliore appareance nei dispositivi mobili.

1. Copia il **Views\Home\Index.cshtml** consente di visualizzare e incollarlo per creare una copia, rinominare il nuovo file **cshtml**.
2. Aprire il nuovo creato **cshtml** consente di visualizzare e sostituire le funzioni &lt;ul&gt; tag con questo codice. In questo modo, aggiornerà il &lt;ul&gt; tag con le annotazioni dei dati per dispositivi mobili di jQuery per usare i temi di jQuery mobili.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Si noti che:
    > 
    > - Il **data-role** attributo impostato su **listview** verrà eseguito il rendering nell'elenco tramite gli stili di listview.
    > 
    > - Il **dati inset** attributo impostato su true sarà visualizzato l'elenco con bordi arrotondati e i margini.
    > 
    > - Il **filtro di dati** attributo impostato su **true** genererà una casella di ricerca.
    > 
    > Altre informazioni sulle convenzioni per jQuery Mobile nella documentazione di progetto: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Premere **CTRL + S** per salvare le modifiche.
4. Passare al **emulatore Windows Phone** e aggiornare il sito. Si noti che il nuovo aspetto dell'elenco della raccolta, nonché la nuova casella di ricerca nella parte superiore. Quindi, digitare una parola nella casella di ricerca (ad esempio, **Tulips**) per avviare la ricerca nella raccolta di foto.

    ![Raccolta con lo stile di listview con i filtri](whats-new-in-aspnet-mvc-4/_static/image25.png "raccolta usando lo stile di listview con i filtri")

    *Raccolta con lo stile di listview con i filtri*

    Per riepilogare, si usa il file recipe Mobilizer vista per creare una copia della visualizzazione Index con la &quot;mobile&quot; suffisso. Questo suffisso indica ad ASP.NET MVC 4 che ogni richiesta generata da un dispositivo mobile userà questa copia dell'indice. Inoltre, è stato aggiornato la versione della visualizzazione Index usare jQuery Mobile per migliorare l'aspetto del sito nei dispositivi mobili per dispositivi mobili.
5. Tornare a Visual Studio e aprire **Site.Mobile.css** sotto il **contenuto** cartella.
6. Correggere il posizionamento del titolo della foto per renderla Mostra sul lato destro dell'immagine. A tale scopo, aggiungere il codice seguente per il **Site.Mobile.css** file.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Premere **CTRL + S** per salvare le modifiche.
8. Tornare alla **emulatore Windows Phone** e aggiornare il sito. Si noti che il titolo di foto è correttamente posizionato a questo punto.

    ![Titolo posizionato sul lato destro dell'immagine](whats-new-in-aspnet-mvc-4/_static/image26.png "titolo posizionato sul lato destro dell'immagine")

    *Titolo posizionato sul lato destro dell'immagine*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Attività 3: temi di jQuery Mobile

Ogni layout e widget in jQuery Mobile viene progettato attorno a un nuovo framework di CSS orientate a oggetti che consente di applicare un tema completo di progettazione visiva unificato per siti e applicazioni.

Per impostazione predefinita del dispositivo Mobile di jQuery tema include i 5 campioni vengono assegnati a lettere (a, b, c, d e) di riferimento rapido.

In questa attività si aggiornerà il layout per dispositivi mobili per l'uso di un tema diverso da quello predefinito.

1. Tornare a Visual Studio.
2. Aprire il  **\_cshtml** che si trova nel file **Views\Shared**.
3. Trovare l'elemento div con il ruolo dati impostato su &quot;pagina&quot; e aggiornare il **data-theme** a &quot; **elettronica**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Premere **CTRL + S** per salvare le modifiche.
5. Aggiornare il sito nel **emulatore Windows Phone** e notare che il nuovo schema di colori.

    ![Layout per dispositivi mobili con una combinazione di colori diversi](whats-new-in-aspnet-mvc-4/_static/image27.png "layout per dispositivi mobili con una combinazione di colori diversi")

    *Layout per dispositivi mobili con una combinazione di colori diversi*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Attività 4: tramite il componente di cambio di visualizzazione e il Browser si esegue l'override di funzionalità

Una convenzione per le pagine web ottimizzata per dispositivi mobili consiste nell'aggiungere un collegamento il cui testo è qualcosa di simile a visualizzazione Desktop o in modalità completa del sito che consente agli utenti di passare a una versione desktop della pagina. Il pacchetto jQuery.Mobile.MVC include un esempio **commutatore della visualizzazione** componente per questo scopo usato nel  **\_cshtml** visualizzazione.

![Collegamento per passare alla visualizzazione Desktop](whats-new-in-aspnet-mvc-4/_static/image28.png "collegamento per passare alla visualizzazione Desktop")

*Collegamento per passare alla visualizzazione Desktop*

Il cambio di visualizzazione viene utilizzata una nuova funzionalità denominata **eseguendo l'override del Browser**. Questa funzionalità consente all'applicazione considerare le richieste come provenienti da un browser (agente utente) diverso da quello da che effettivamente provengano.

In questa attività si esploreranno l'implementazione di esempio di una selezione di vista aggiunta da jQuery.Mobile.MVC e il nuovo browser si esegue l'override di funzionalità in ASP.NET MVC 4.

1. Tornare a Visual Studio.
2. Aprire il  **\_cshtml** vista che si trova sotto il **Views\Shared** cartella e notare che il componente di cambio di visualizzazione cui viene fatto riferimento come visualizzazione parziale.

    ![Layout per dispositivi mobili utilizzando il componente di cambio di visualizzazione](whats-new-in-aspnet-mvc-4/_static/image29.png "layout per dispositivi mobili utilizzando il componente di cambio di visualizzazione")

    *Layout per dispositivi mobili utilizzando il componente di cambio di visualizzazione*
3. Aprire il  **\_ViewSwitcher.cshtml** visualizzazione parziale.

    La visualizzazione parziale viene utilizzato il nuovo metodo **ViewContext.HttpContext.GetOverriddenBrowser()** per determinare l'origine della richiesta web e visualizzare il collegamento corrispondente per passare alle visualizzazioni per dispositivi mobili o Desktop.

    Il **GetOverridenBrowser** metodo restituisce un' **HttpBrowserCapabilitiesBase** istanza che corrisponde all'agente utente attualmente impostato per la richiesta (effettivo o sottoposto a override). È possibile utilizzare questo valore per ottenere le proprietà, ad esempio **IsMobileDevice**.

    ![Visualizzazione parziale ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher visualizzazione parziale")

    *Visualizzazione parziale ViewSwitcher*
4. Aprire il **ViewSwitcherController.cs** classe si trova nel **controller** cartella. Tale azione SwitchView check-out viene chiamato dal collegamento sotto il componente ViewSwitcher e notare i nuovi metodi HttpContext.

    - Il **HttpContext.ClearOverridenBrowser()** metodo rimuove qualsiasi agente utente sottoposto a override per la richiesta corrente.
    - Il **HttpContext.SetOverridenBrowser()** metodo esegue l'override del valore della richiesta degli utenti effettivi dell'agente tramite l'agente utente specificato.  
        ![Controller ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")  
*ViewSwitcher Controller*

        Override del browser è una funzionalità fondamentale di ASP.NET MVC 4, anch ' esso disponibile anche se non si installa il pacchetto jQuery.Mobile.MVC. Tuttavia, questa funzionalità interessa solo visualizzazione, layout e visualizzazione parziale e non influenza le funzionalità che dipendono dall'oggetto Request.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Attività 5: aggiunta al commutatore della visualizzazione nella vista del Desktop

In questa attività si aggiornerà il layout desktop per includere il cambio di visualizzazione. Ciò consentirà agli utenti di dispositivi mobili tornare alla visualizzazione per dispositivi mobili quando si esplorano la visualizzazione desktop.

1. Aggiornare il sito nel **emulatore Windows Phone**.
2. Fare clic sui **visualizzazione Desktop** collegamento all'inizio della raccolta. Si noti che non vi è alcun commutatore della visualizzazione nella vista del desktop per consentire che tornare alla visualizzazione per dispositivi mobili.
3. Tornare a Visual Studio e aprire il  **\_layout. cshtml** visualizzazione.
4. Trovare la sezione di accesso e inserire una chiamata per eseguire il rendering di  **\_ViewSwitcher** visualizzazione parziale riportato di seguito il  **\_LogOnPartial** visualizzazione parziale. Quindi, premere **CTRL + S** per salvare le modifiche.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Premere **CTRL + S** per salvare le modifiche.
6. Aggiornare la pagina nell'emulatore Windows Phone e fare doppio clic sulla schermata per fare zoom avanti. Si noti che la home page viene ora illustrato il **visualizzazione per dispositivi mobili** collegamento che passa da dispositivi mobili per la visualizzazione desktop.

    ![Visualizzazione della selezione viene eseguito il rendering nella visualizzazione desktop](whats-new-in-aspnet-mvc-4/_static/image32.png "cambio di visualizzazione viene eseguito il rendering nella visualizzazione desktop")

    *Cambio di visualizzazione viene eseguito il rendering nella visualizzazione desktop*
7. Passare nuovamente alla visualizzazione per dispositivi mobili e passare a <strong>sulle</strong> pagina (http://localhost[porta] / Home/su). Si noti che, anche se è stata creata una vista About.Mobile.cshtml, la pagina di informazioni viene visualizzata utilizzando il layout per dispositivi mobili (\_cshtml).

    ![Informazioni sulla pagina](whats-new-in-aspnet-mvc-4/_static/image33.png "sulla pagina")

    *Informazioni sulla pagina*
8. Infine, aprire il sito in un browser desktop. Si noti che nessuno degli aggiornamenti precedenti ha modificato la visualizzazione desktop.

    ![Visualizzazione desktop PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "visualizzazione desktop PhotoGallery")

    *Visualizzazione desktop PhotoGallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Attività 6: creazione di nuove modalità di visualizzazione

La nuova funzionalità di modalità di visualizzazione consente a un'applicazione di selezionare le viste in base al browser che genera la richiesta. Ad esempio, se un browser desktop richiede la Home page, l'applicazione verrà restituito il **Views\Home\Index.cshtml** modello. Quindi, se un browser per dispositivi mobili richiede la Home page, l'applicazione verrà restituito il **Views\Home\Index.mobile.cshtml** modello.

In questa attività si creerà un layout personalizzato per i dispositivi iPhone e si dovrà simulare le richieste provenienti da dispositivi iPhone. A tale scopo, è possibile usare entrambi un emulatore o simulatore iPhone (ad esempio [Electric simulatore di dispositivi mobili](http://www.electricplum.com/)) o un browser con i componenti aggiuntivi che modificano l'agente utente. Per istruzioni su come impostare la stringa agente utente in un browser Safari per emulare un iPhone, vedere [come consentire Safari fingono è IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) nel blog di David Alison.

**Si noti che questa attività è facoltativa ed è possibile continuare nell'intero laboratorio senza eseguirla.**

1. In Visual Studio, premere **SHIFT** + **F5** per arrestare il debug dell'applicazione.
2. Aprire **Global.asax.cs** e aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Aggiungere il codice evidenziato seguente all'applicazione\_Start (metodo).

    (Code - Snippet *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

È stato registrato un nuovo **DefaultDisplayMode denominato &quot;iPhone&quot;**, all'interno statico **DisplayModeProvider.Instance.Modes** elenco statico, che verrà confrontata con ogni richiesta in ingresso. Se la richiesta in ingresso contiene la stringa &quot;iPhone&quot;, ASP.NET MVC troverà le visualizzazioni il cui nome contiene la &quot;iPhone&quot; suffisso. Il parametro 0 indica la modalità specifica è la nuova modalità; ad esempio, questa vista è più specifica generali &quot;. Mobile&quot; regola corrispondente le richieste provenienti da dispositivi mobili.

Quando si esegue questo codice, quando si genera una richiesta di un browser di iPhone, l'applicazione userà il **Views\Shared\\_Layout.iPhone.cshtml** layout creerai nei passaggi successivi.

> [!NOTE]
> In questo modo, la richiesta di test per iPhone è stato semplificato a scopo dimostrativo e potrebbe non funzionare come previsto per ogni stringa di agente utente iPhone (per il test di esempio è distinzione maiuscole / minuscole).

4. Creare una copia del  **\_cshtml** del file nei **Views\Shared** cartella e rinominare la copia &quot; **\_Layout.iPhone.csthml**&quot;.
5. Aprire  **\_Layout.iPhone.csthml** creato nel passaggio precedente.
6. Trovare l'elemento div con l'attributo data-role impostato **pagina** e modificare le **data-theme** dell'attributo &quot; **un**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Ora si dispone di 3 layout nell'applicazione ASP.NET MVC 4:

1. **\_Layout. cshtml**: layout predefinito utilizzato per i browser desktop.
2. **\_Cshtml**: layout predefinito utilizzato per i dispositivi mobili.
3. **\_Cshtml**: un layout specifico per i dispositivi iPhone, usando una combinazione di colori diversi per differenziare da \_cshtml.
7. Premere **F5** per eseguire l'applicazione ed esplorare il sito nel **emulatore Windows Phone**.
8. Aprire una **simulatore iPhone** (vedere [appendice C](#AppendixC) per istruzioni su come installare e configurare un simulatore di iPhone) e passare al sito troppo. Si noti che ogni telefono utilizza il modello specifico.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Utilizzo di visualizzazioni diverse per ogni dispositivo mobile*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Esercizio 4: Utilizzo di controller asincroni

Microsoft .NET Framework 4.5 introduce nuove funzionalità del linguaggio in c# e Visual Basic per fornire una nuova base per la modalità asincrona nella programmazione .NET. Questa nuova base rende la programmazione asincrona simile a - e circa semplice come - programmazione sincrona. Questo punto si è in grado di scrivere metodi di azione asincroni in ASP.NET MVC 4 usando il **AsyncController** classe. È possibile usare metodi di azione asincroni per a esecuzione prolungata, le richieste associate alla CPU non. Questo evita il server Web di esecuzione del lavoro mentre viene elaborata la richiesta di blocco. La classe AsyncController è in genere usata per le chiamate al servizio Web con esecuzione prolungata.

Questo esercizio illustra i concetti di base dell'operazione asincrona in ASP.NET MVC 4. Se si desiderano ulteriori approfondimenti, è possibile consultare l'articolo seguente: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Attività 1: implementazione di un Controller asincrono

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex4-Async/inizio/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **HomeController.cs** classe la **controller** cartella.
3. Aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Aggiorna il **HomeController** classe da cui ereditare **AsyncController**. Controller che derivano da AsyncController abilitare ASP.NET elaborare le richieste asincrone e possono comunque i metodi di azione sincrono di servizio.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Aggiungere il **async** parola chiave per il **indice** (metodo) e impostarlo come il tipo restituito **attività&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Il **async** (parola chiave) è uno delle nuove parole chiave fornisce .NET Framework 4.5, indica al compilatore che questo metodo contiene codice asincrono. Oggetto **attività** oggetto rappresenta un'operazione asincrona che può essere completato in un determinato momento nel futuro.
6. Sostituire il **client. GetAsync()** chiamata con la versione asincrona completo usando parola chiave await come illustrato di seguito.

    (Code - Snippet *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Nella versione precedente, si usava la **risultato** proprietà dalle **attività** oggetto da bloccare il thread fino a quando non viene restituito il risultato (versione di sincronizzazione).
    > 
    > Aggiungere il **await** parola chiave indica al compilatore di attendere in modo asincrono l'attività restituita dalla chiamata al metodo. Ciò significa che il resto del codice verrà eseguito come una richiamata solo dopo che il metodo di atteso viene completata. Un'altra cosa da notare è che non è necessario modificare il blocco try-catch per ottenere questo risultato: le eccezioni che si verificano in background o in primo piano continuano a essere intercettate senza operazioni aggiuntive utilizzando un gestore fornito dal framework.
7. Modificare il codice per continuare con l'implementazione asincrona, sostituendo le righe con il nuovo codice, come illustrato di seguito

    (Code - Snippet *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Eseguire l'applicazione. Non si noterà alcuna modifica significativa, ma il codice non bloccherà un thread dal pool di thread, rendendo un migliore utilizzo delle risorse del server e migliorando le prestazioni.

    > [!NOTE]
    > Altre informazioni sulle nuove funzionalità di programmazione asincrona nel laboratorio &quot; **programmazione asincrona in .NET 4.5 con c# e Visual Basic** &quot; incluse nel Training Kit di Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Attività 2: la gestione dei timeout con i token di annullamento

Metodi di azione asincroni che restituiscono istanze delle attività possono supportare anche valori di timeout. In questa attività si aggiornerà il codice del metodo dell'indice per gestire uno scenario di timeout usando un token di annullamento.

1. Tornare a Visual Studio e premere **MAIUSC+F5** per arrestare il debug.
2. Aggiungere la seguente istruzione using il **HomeController.cs** file.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Aggiornare l'azione di indice per ricevere una **CancellationToken** argomento.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Aggiorna il **GetAsync** chiamata per passare il token di annullamento.

    (Code - Snippet *SendAsync Lab - Ex04 - ASP.NET MVC 4 con CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decorare il *indice* metodo con un **AsyncTimeout** attributo impostato su 500 millisecondi e un **HandleError** attributo è configurato per gestire  **TaskCanceledException** mediante il reindirizzamento a un **TimedOut** visualizzazione.

    (Code - Snippet *ASP.NET MVC 4 Lab - Ex04 - attributi*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Aprire il **PhotoController** classe e l'aggiornamento di **raccolta** metodo per ritardare l'esecuzione 1000 millisecondi (1 secondo) per simulare un'attività a esecuzione prolungata.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Aprire il **Web. config** file e abilitare gli errori personalizzati, aggiungere l'elemento seguente.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Creare una nuova visualizzazione nel **Views\Shared** denominate **TimedOut** e usare il layout predefinito. In Esplora soluzioni fare doppio clic il **Views\Shared** cartella e selezionare **Add | Visualizzazione**.

    ![Utilizzo di visualizzazioni diverse per ogni dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image36.png "con visualizzazioni diverse per ogni dispositivo mobile")

    *Utilizzo di visualizzazioni diverse per ogni dispositivo mobile*
9. Aggiorna il **TimedOut** visualizzare il contenuto come illustrato di seguito.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Eseguire l'applicazione e passare all'URL radice. Come è stato aggiunto un **thread. Sleep** di 1000 millisecondi, si otterrà un errore di timeout, generato dal **AsyncTimeout** attributo e intercettare per il **HandleError** attributo.

    ![Eccezione di timeout gestita](whats-new-in-aspnet-mvc-4/_static/image37.png "gestita l'eccezione di timeout")

    *Eccezione di timeout gestita*

> [!NOTE]
> Inoltre, è possibile distribuire questa applicazione per siti Web di Azure seguenti [appendice d: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo-pratica, è stato osservato alcune delle nuove funzionalità residenti in ASP.NET MVC 4. Sono stati trattati i concetti seguenti:

- Sfruttare i vantaggi dei miglioramenti apportati a di ASP.NET MVC progetto modelli, che comprende il nuovo modello di progetto di applicazione per dispositivi mobili
- Utilizzare l'attributo viewport HTML5 e query sui supporti CSS per migliorare la visualizzazione su dispositivi mobili
- Usare jQuery Mobile per i miglioramenti progressivi e la creazione di web touchscreen ottimizzata dell'interfaccia utente
- Creazione di visualizzazioni specifiche per dispositivi mobili
- Utilizzare il componente di cambio di visualizzazione per passare tra le visualizzazioni per dispositivi mobili e desktop nell'applicazione
- Creare controller asincrono usando il supporto di attività

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Appendice a: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](whats-new-in-aspnet-mvc-4/_static/image38.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](whats-new-in-aspnet-mvc-4/_static/image39.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](whats-new-in-aspnet-mvc-4/_static/image40.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](whats-new-in-aspnet-mvc-4/_static/image41.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)***

1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.
2. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
3. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](whats-new-in-aspnet-mvc-4/_static/image42.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](whats-new-in-aspnet-mvc-4/_static/image43.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Appendice b: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

    ![Installa Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Installazione completata*
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Visual Studio Express per il riquadro Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Appendice c: installazione di WebMatrix 2 e simulatore iPhone

Per eseguire il sito in un dispositivo simulato iPhone è possibile usare l'estensione di WebMatrix &quot;Electric simulatore di dispositivi mobili per iPhone&quot;. Inoltre, è possibile configurare la stessa estensione per eseguire il simulatore di Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Attività 1: installazione di WebMatrix 2

1. Passare a [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>WebMatrix 2</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

    ![Installare WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "installare WebMatrix 2")

    *Installare WebMatrix 2*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](whats-new-in-aspnet-mvc-4/_static/image50.png "accettando le condizioni di licenza")

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Lo stato dell'installazione](whats-new-in-aspnet-mvc-4/_static/image51.png "lo stato dell'installazione")

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-aspnet-mvc-4/_static/image52.png "installazione è stata completata")

    *Installazione completata*
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Attività 2: installare l'estensione di simulatore iPhone

1. Eseguire **WebMatrix** e aprire qualsiasi sito Web esistente o crearne uno nuovo.
2. Fare clic sul **eseguiti** pulsante la **Home** della barra multifunzione e selezionare **Aggiungi nuovo**.

    ![Aggiunta nuova estensione di WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "aggiunta nuova estensione di WebMatrix")

    *Aggiunta nuova estensione di WebMatrix*
3. Selezionare **simulatore iPhone** e fare clic su **installare**.

    ![Esplorazione delle estensioni di WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "estensioni esplorazione WebMatrix")

    *Esplorazione delle estensioni di WebMatrix*
4. Nei dettagli del pacchetto, fare clic su **installare** per continuare con l'installazione dell'estensione.

    ![estensione di simulatore iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "estensione simulatore iPhone")

    *estensione di simulatore iPhone*
5. Leggere e accettare le condizioni di licenza di estensione.

    ![Estensione di WebMatrix EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "con l'estensione di WebMatrix")

    *Con l'estensione di WebMatrix*
6. A questo punto, è possibile eseguire il sito Web da WebMatrix utilizzando l'opzione simulatore iPhone.

    ![Esecuzione con un iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "eseguiti usando iPhone")

    *Esecuzione con un iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Attività 3: configurazione di Visual Studio 2012 per l'esecuzione di simulatore iPhone

1. Aprire **Visual Studio 2012** e aprire qualsiasi sito Web o creare un nuovo progetto.
2. Fare clic sulla freccia verso il basso sul pulsante di esecuzione e selezionare **esplorare con**.

    ![Esplora con](whats-new-in-aspnet-mvc-4/_static/image58.png "Esplora con")

    *Esplora con*
3. Nel &quot;Esplora con&quot; finestra di dialogo, fare clic su **Add**.
4. Nel &quot;Aggiungi programma&quot; finestra di dialogo, usare i valori seguenti:

   - <strong>Programma</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (aggiornare di conseguenza il percorso)</em>
   - **Gli argomenti**: &quot;1&quot;
   - **Nome descrittivo**: simulatore iPhone

     ![Aggiungi programma](whats-new-in-aspnet-mvc-4/_static/image59.png "Aggiungi programma")

     *Aggiungi programma di esplorare con*
5. Fare clic su **OK** e chiudere le finestre di dialogo.
6. A questo punto si è in grado di eseguire le applicazioni Web nel simulatore iPhone da Visual Studio 2012.

    ![Esplora con simulatore iPhone](whats-new-in-aspnet-mvc-4/_static/image60.png "Sfoglia con simulatore iPhone")

    *Esplora con simulatore iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice d: la pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

In questa appendice spiega come creare un nuovo sito web dal portale di gestione di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1: creazione di un nuovo sito Web di Windows portale di Azure

1. Andare alla [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico. È possibile effettuare l'iscrizione [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale di Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "accedere al portale di Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **New** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-aspnet-mvc-4/_static/image62.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **Compute** | **sito Web**. Quindi selezionare **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure all'esterno del portale. Non include i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-aspnet-mvc-4/_static/image63.png "creando un nuovo sito Web mediante Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna. Verificare che il nuovo sito Web sia in funzione.

    ![Passare al nuovo sito web](whats-new-in-aspnet-mvc-4/_static/image64.png "passare al nuovo sito web")

    *Passare al nuovo sito web*

    ![Sito Web in esecuzione](whats-new-in-aspnet-mvc-4/_static/image65.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione sito web](whats-new-in-aspnet-mvc-4/_static/image66.png "aprire le pagine di gestione sito web")

    *Aprire le pagine di gestione sito Web*
7. Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in siti Web di Windows Azure.

    ![Download del sito web di profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image67.png "scaricando il sito web di profilo di pubblicazione")

    *Profilo di pubblicazione scaricato il sito Web*
8. Scaricare il file di profilo di pubblicazione in una posizione nota. In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file di profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image68.png "salvataggio del profilo di pubblicazione")

    *Salvare il file di profilo di pubblicazione*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server. Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi. Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard di Server di Database SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "Dashboard di Server di Database SQL")

    *Dashboard di Server di Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) pulsante.

    ![Aggiunta indirizzo IP del Client](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confermare le modifiche*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.

    ![Pubblicazione dell'applicazione](whats-new-in-aspnet-mvc-4/_static/image73.png "pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image74.png "l'importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Dopo aver completata la convalida fare clic su **successivo**.

    > [!NOTE]
    > La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.

    ![La convalida connessione](whats-new-in-aspnet-mvc-4/_static/image75.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).

    ![Configurazione della distribuzione Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.
   - Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.
   - Nelle **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione di stringa di connessione di destinazione](whats-new-in-aspnet-mvc-4/_static/image77.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](whats-new-in-aspnet-mvc-4/_static/image78.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **Preview** pagina, fare clic su **Publish**.

    ![Pubblicazione dell'applicazione web](whats-new-in-aspnet-mvc-4/_static/image80.png "pubblicazione dell'applicazione web")

    *Pubblicazione dell'applicazione web*
9. Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "applicazione pubblicata in Windows Azure")

    *Pubblicata dell'applicazione in Windows Azure*
