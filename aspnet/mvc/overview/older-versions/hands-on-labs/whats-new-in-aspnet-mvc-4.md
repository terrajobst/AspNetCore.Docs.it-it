---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "Novità di ASP.NET MVC 4 | Documenti Microsoft"
author: rick-anderson
description: "ASP.NET MVC 4 è un framework per la compilazione di applicazioni web scalabili e basate su standard utilizzando schemi progettuali ben definiti e le potenzialità di ASP.NET e..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 35f9402ad6090c0441425a23b2b8063350892210
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Novità di ASP.NET MVC 4

Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 è un framework per la compilazione di applicazioni web scalabili e basate su standard utilizzando schemi progettuali ben definiti e le potenzialità di ASP.NET e .NET framework. Questa nuova, quarta versione di framework è incentrata su come rendere più semplice lo sviluppo di applicazioni web per dispositivi mobili.

Per iniziare, quando si crea un nuovo progetto ASP.NET MVC 4 è ora disponibile un modello di progetto di applicazione per dispositivi mobili che è possibile utilizzare per creare un'applicazione autonoma in particolare per i dispositivi mobili. Inoltre, ASP.NET MVC 4 si integra con jQuery Mobile tramite un pacchetto NuGet jQuery.Mobile.MVC. jQuery Mobile è un framework basato su HTML5 per lo sviluppo di applicazioni web compatibili con tutte le piattaforme dei dispositivi mobili più comuni, tra cui Windows Phone, iPhone, Android e così via. Tuttavia, se è necessario specializzazione, ASP.NET MVC 4 consente inoltre di fornire visualizzazioni diverse per diversi dispositivi e specificare le ottimizzazioni specifiche del dispositivo.

In questa esercitazione pratica, si inizierà con ASP.NET MVC 4 &quot;applicazione Internet&quot; modello di progetto per creare un'applicazione Raccolta foto. Si procederà al miglioramento progressivamente l'app usando jQuery Mobile e nuove funzionalità di ASP.NET MVC 4 per assicurarsi che sia compatibile con diversi dispositivi mobili e browser desktop. Si apprenderà inoltre nuove soluzioni di codice per la generazione di codice e come ASP.NET MVC 4 rende più semplice per la scrittura di metodi di azione asincroni grazie al supporto delle attività&lt;ActionResult&gt; tipi restituiti.

> [!NOTE]
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico per questa esercitazione è disponibile all'indirizzo [novità di Web Form in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Sfruttare i miglioramenti per i modelli, compresi progetto di MVC ASP.NET il nuovo modello di progetto di applicazione per dispositivi mobili
- Utilizzare le query di supporto CSS e attributo viewport HTML5 per migliorare la visualizzazione nei dispositivi mobili
- Utilizzare jQuery Mobile per l'ottimizzazione progressivi e per la compilazione di interfaccia utente web ottimizzata per il tocco
- Creare visualizzazioni specifiche di dispositivi mobili
- Utilizzare il componente di selezione di visualizzazione per passare tra le visualizzazioni di dispositivi mobili e desktop nell'applicazione
- Creare i controller asincroni utilizzando il supporto delle attività

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

È necessario che gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice B](#AppendixB) per istruzioni su come installarlo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluso nell'installazione di Microsoft Visual Studio 2012)
- Emulatore Windows Phone (incluso nel [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Facoltativo: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con **Electric Plum iPhone simulatore** estensione (solo per esercizio 3 consente di visualizzare l'applicazione web con un simulatore di iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

In tutto il documento lab, verrà invitati a inserire i blocchi di codice. Per praticità, la maggior parte di tale codice viene fornito come Visual Studio frammenti di codice che è possibile utilizzare da Visual Studio per evitare di dover aggiungerla manualmente.

Per installare i frammenti di codice:

1. Aprire una finestra di Esplora risorse e individuare il lab **Source\Setup** cartella.
2. Fare doppio clic su di **Setup.cmd** file in questa cartella per installare i frammenti di codice di Visual Studio.

Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice a: con i frammenti di codice](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione include gli esercizi seguenti:

1. [Nuovi modelli di progetto ASP.NET MVC 4](#Exercise1)
2. [Creazione dell'applicazione Web Raccolta foto](#Exercise2)
3. [Aggiunta del supporto per i dispositivi mobili](#Exercise3)
4. [Utilizzo di un controller asincrono](#Exercise4)

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Esercizio 1: Nuovi modelli di progetto ASP.NET MVC 4

In questo esercizio verranno esplorati i miglioramenti nei modelli di progetto ASP.NET MVC 4. Oltre al modello di applicazione Internet, è già presente in MVC 3, questa versione include ora un modello distinto per le applicazioni mobili. In primo luogo, esaminerà alcune funzionalità rilevanti di ognuno dei modelli. Quindi, verranno usati per il rendering della pagina correttamente su piattaforme diverse utilizzando l'approccio giusto.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Attività 1: esplorare il modello di applicazione Internet

1. Aprire **Visual Studio**.
2. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere **applicazione Web ASP.NET MVC 4.** Denominare il progetto **PhotoGallery**, selezionare un percorso (o lasciare il valore predefinito) e fare clic su **OK**.

    > [!NOTE]
    > Successivamente si personalizzerà la soluzione PhotoGallery ASP.NET MVC 4 che ora si sta creando.

    ![Crea un nuovo progetto](whats-new-in-aspnet-mvc-4/_static/image1.png "creando un nuovo progetto")

    *Crea un nuovo progetto*
3. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona il **applicazione Internet** modello di progetto e fare clic su **OK**. Verificare che sia selezionato Razor come motore di visualizzazione.

    ![Crea una nuova applicazione Internet ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "crea una nuova applicazione Internet ASP.NET MVC 4")

    *Crea una nuova applicazione Internet ASP.NET MVC 4*

    > [!NOTE]
    > La sintassi Razor è stato introdotto in ASP.NET MVC 3. L'obiettivo consiste nel ridurre al minimo il numero di caratteri e sequenze di tasti richieste in un file, consentendo un rapido e fluido codifica del flusso di lavoro. Sfrutta Razor esistente c# / VB (o altri) linguistiche e offre una sintassi di modello che consente un straordinario HTML costruzione del flusso di lavoro.
4. Premere **F5** per eseguire la soluzione e visualizzare i modelli rinnovati. È possibile controllare le funzionalità seguenti:

    **Modelli di stile moderno**

    I modelli sono stati rinnovati, fornendo più stili dall'aspetto moderno.

    ![Modelli ASP.NET MVC 4 modificato nello stile](whats-new-in-aspnet-mvc-4/_static/image3.png "modificato nello stile di modelli di MVC 4")

    *Modelli ASP.NET MVC 4 modificato nello stile*

    ![Nuova pagina contatto](whats-new-in-aspnet-mvc-4/_static/image4.png "pagina nuovo contatto")

    *Nuova pagina di contatto*

    **Rendering adattivo**

    Estrarre il ridimensionamento della finestra del browser e notare come il layout di pagina si adatta dinamicamente per le nuove dimensioni della finestra. Questi modelli utilizzano la tecnica di rendering adattivo per eseguire correttamente il rendering in piattaforme mobili e desktop senza personalizzazione.

    ![Modello di progetto ASP.NET MVC 4 in formati diversi browser](whats-new-in-aspnet-mvc-4/_static/image5.png "il modello di progetto ASP.NET MVC 4 in formati diversi browser")

    *Modello di progetto ASP.NET MVC 4 in formati diversi browser*

    **Interfaccia utente più completi con JavaScript**

    Un altro miglioramento di modelli di progetto predefiniti è l'utilizzo di JavaScript per fornire un maggior livello di interattività JavaScript. I collegamenti di accesso e registrazione utilizzati nel modello esemplificare utilizzare jQuery convalide per convalidare i campi di input dal lato client.

    ![Convalida jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    Convalida jQuery

    > [!NOTE]
    > Si noti che i due log nelle sezioni, nella prima sezione è possono accedere utilizzando un account registrato dal sito e nella seconda sezione che è possibile altenativelly l'accesso utilizzando un altro servizio di autenticazione come google (disattivato per impostazione predefinita).
5. Chiudere il browser per arrestare il debugger e tornare a Visual Studio.
6. Aprire il file **AuthConfig.cs** sotto il **App\_avviare** cartella.
7. Rimuovere il commento dall'ultima riga per registrare il client di Google per *OAuth* l'autenticazione.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Si noti che è possibile abilitare facilmente l'autenticazione utilizzando qualsiasi servizio OpenID o OAuth come Facebook, Twitter, Microsoft, e così via.
8. Premere **F5** per eseguire la soluzione e passare alla pagina di accesso.
9. Selezionare **Google** servizio per l'accesso.

    ![Selezione del log nel servizio](whats-new-in-aspnet-mvc-4/_static/image7.png)

    Selezione del log nel servizio
10. Accedere con l'account di Google.
11. Consentire al sito (localhost) recuperare informazioni dall'account di Google.
12. Infine, è necessario registrare il sito per associare l'account di Google.

    ![Associare l'account Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

    *Associare l'account Google*
13. Chiudere il browser per arrestare il debugger e tornare a Visual Studio.
14. Esaminare ora la soluzione di estrarre alcune altre nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.

    ![Il modello di progetto applicazione ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "il modello di progetto ASP.NET MVC 4 applicazione Internet")

    *Il modello di progetto ASP.NET MVC 4 applicazione Internet*

    - **HTML 5 Markup**

        Sfogliare le viste di modello per individuare il nuovo tag di tema.

        ![Nuovo modello, utilizzando il markup About.cshtml Razor e HTML5. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Nuovo modello, utilizzando il markup About.cshtml Razor e HTML5.")

        *Nuovo modello, utilizzando il markup Razor e HTML5 (About.cshtml).*
    - **Librerie JavaScript aggiornate**

        Il modello predefinito di ASP.NET MVC 4 include ora Knockout.js e un framework JavaScript MVVM che consente di creare applicazioni web ad alte utilizzando JavaScript e HTML. Ad esempio in MVC 3, jQuery e jQuery librerie di interfaccia utente sono incluse anche in ASP.NET MVC 4.

        > [!NOTE]
        > È possibile ottenere ulteriori informazioni sulla libreria Knockout.js in questo collegamento: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). Inoltre, è possibile acquisire informazioni jQuery e jQuery UI in [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Attività 2: esplorare il modello di applicazione per dispositivi mobili

ASP.NET MVC 4 facilita lo sviluppo di siti Web per dispositivi mobili e tablet browser. Questo modello ha la stessa struttura dell'applicazione come modello di applicazione Internet (si noti che il codice del controller è praticamente identico), ma lo stile è stato modificato per eseguire il rendering correttamente i dispositivi mobili basati su tocco.

1. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere il **applicazione Web ASP.NET MVC 4.** Denominare il progetto **PhotoGallery.Mobile**, selezionare un percorso (o lasciare l'impostazione predefinita), selezionare &quot;aggiungere alla soluzione&quot; e fare clic su **OK**.
2. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona il **applicazione Mobile** modello di progetto e fare clic su **OK**. Verificare che sia selezionato Razor come motore di visualizzazione.

    ![Crea una nuova applicazione Mobile ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "crea una nuova applicazione Mobile ASP.NET MVC 4")

    *Crea una nuova applicazione Mobile ASP.NET MVC 4*
3. Si è ora possibile esplorare la soluzione e verificare alcune delle nuove funzionalità introdotte dal modello di soluzione ASP.NET MVC 4 per dispositivi mobili:

    - **jQuery Mobile libreria**

        Il modello di progetto di applicazione per dispositivi mobili include la libreria jQuery Mobile, che è una libreria open source per la compatibilità browser per dispositivi mobili. jQuery Mobile applica progressivo miglioramento per il browser per dispositivi mobili che supportano CSS e JavaScript. Progressivo miglioramento consente a tutti i browser visualizzare il contenuto di base di una pagina web, mentre consente solo i browser più potenti visualizzare il contenuto RTF. I file JavaScript e CSS, inclusi in jQuery Mobile stile, consentono di browser per dispositivi mobili per adattare il contenuto nella schermata senza apportare modifiche nel markup della pagina.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *libreria di jQuery mobile inclusa nel modello*
    - **Markup basato su HTML5**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Modello di applicazione per dispositivi mobili tramite markup HTML5, (cshtml e cshtml)*
4. Premere **F5** per eseguire la soluzione.
5. Aprire il **Windows Phone 7 emulatore**.
6. Nella schermata start telefono, aprire Internet Explorer. Estrarre l'URL in cui l'applicazione desktop di avvio e passare a tale URL dal telefono (ad esempio `http://localhost:[PortNumber]/`).
7. È ora in grado di accedere alla pagina di accesso o estrarre le informazioni sulla pagina. Si noti che lo stile del sito Web è basato sulla nuova app di Windows Store per dispositivi mobili. Il modello di progetto ASP.NET MVC 4 sia visualizzato correttamente sui dispositivi mobili, assicurarsi che tutti gli elementi della pagina sono visibili e abilitati. Si noti che i collegamenti nell'intestazione sono sufficientemente grandi per essere selezionato o scelte.

    ![Progetto pagine modello in un dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image14.png "progetto pagine modello in un dispositivo mobile")

    *Pagine modello di progetto in un dispositivo mobile*
8. Utilizza inoltre il nuovo modello di **tag meta Viewport**. Browser per più dispositivi mobili definire una larghezza di una finestra del browser virtuale o &quot;viewport&quot;, che è maggiore della larghezza effettiva del dispositivo mobile. In questo modo il browser per dispositivi mobili visualizzare l'intera pagina web all'interno di visualizzazione virtuale. Il **tag meta Viewport** consente agli sviluppatori web impostare la larghezza, altezza e la scala dell'area del browser per dispositivi mobili **.** Il modello ASP.NET MVC 4 per applicazioni per dispositivi mobili imposta il viewport per la larghezza del dispositivo (&quot;larghezza = dispositivo larghezza&quot;) nel modello di layout (*Views\Shared\_cshtml*), in modo che tutti i le pagine verranno impostati i viewport alla larghezza dello schermo di dispositivi. Si noti che il tag meta del riquadro di visualizzazione non cambierà la visualizzazione del browser predefinito.
9. Aprire  **\_cshtml**, all'interno di **viste | Condiviso** cartella e commento per il tag meta Viewport. Eseguire l'applicazione, se non è già aperto ed estrarre le differenze.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    ![Il sito dopo l'impostazione come commento il tag meta viewport](whats-new-in-aspnet-mvc-4/_static/image15.png "il sito dopo il tag meta viewport ai commenti")

    *Il sito dopo il tag meta viewport ai commenti*
10. In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.
11. Rimuovere il tag meta viewport.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Attività 3: usare il Rendering adattivo

In questa attività si apprenderà a un altro metodo per eseguire il rendering di una pagina Web correttamente nei dispositivi mobili e browser Web nello stesso momento senza personalizzazione. Tag meta Viewport è già stato utilizzato con uno scopo simile. Ora è grado di soddisfare un altro metodo potente: *rendering adattivo*.

Il rendering adattivo è una tecnica che utilizza **query di CSS3 media** per personalizzare lo stile applicato a una pagina. Query supporti definire condizioni all'interno di un foglio di stile, il raggruppamento di stili CSS in una determinata condizione. Solo quando la condizione è true, lo stile viene applicato all'oggetto dichiarato.

La flessibilità offerta dalla tecnica di rendering adattivo consente qualsiasi personalizzazione per la visualizzazione del sito in dispositivi diversi. È possibile definire gli stili tante desiderato in un unico foglio di stile senza scrivere codice di logica per scegliere lo stile. Pertanto, è un modo molto semplice di adattamento di stili di pagina, in quanto riduce la quantità di codice duplicato e la logica per ragioni di rendering. D'altra parte, il consumo di larghezza di banda aumenterebbe, come le dimensioni dei file CSS potrebbero aumentare leggermente.

Utilizzando la tecnica di rendering adattivo, sarà il sito **visualizzato correttamente, indipendentemente dal browser.** Tuttavia, è consigliabile se la larghezza di banda aggiuntiva carica rappresenta un problema.

> [!NOTE]
> È il formato di base di una query supporti: @media \[ambito: tutti | palmari | stampare | proiezione | schermo\] ([proprietà: valore] e... [proprietà: valore])


Esempi di query di supporto: &gt;  **@media tutti e (larghezza massima: 1000px) e (min-width: 700px) {}:** per tutte le risoluzioni tra 700px e 1000px.

> **@media schermata e (min-width: 400px) e (larghezza massima: 700px) {…}:** solo per le schermate. La risoluzione deve essere compreso tra 400 e 700px.
> 
> **@media palmari e (min-width: 20em), schermo e (min-width: 20em) {…}:** per palmari (mobili e dispositivi) e schermate iniziali. La larghezza minima deve essere maggiore di 20em.
> 
> È possibile trovare ulteriori informazioni sul [sito W3C](http://www.w3.org/TR/css3-mediaqueries/).


Verranno ora analizzate funzionamento il rendering, migliorando la leggibilità di ASP.NET MVC 4 predefiniti modello di sito Web.

1. Aprire il **PhotoGallery.sln** soluzione è stato creato nel passaggio 1 e selezionare il **PhotoGallery** progetto. Premere **F5** per eseguire la soluzione.
2. Ridimensionare la larghezza del browser, l'impostazione di windows per la metà o meno di un quarto della dimensione originale. Notare cosa accade con gli elementi nell'intestazione: alcuni elementi non saranno visualizzati nell'area visibile dell'intestazione.
3. Aprire **Site.css** file da Esplora soluzioni di Visual Studio, si trova **contenuto** cartella del progetto. Premere **CTRL + F** aprire ricerca integrata in Visual Studio e scrivere  **@media**  per individuare il **query supporti CSS**.

    La condizione di query supporti definita in questo modello funziona in questo modo: quando sono di dimensioni della finestra del browser sotto **850 px**, le regole CSS applicate sono quelli definiti all'interno del blocco di supporto.

    ![Individuare la query di supporto](whats-new-in-aspnet-mvc-4/_static/image16.png "individuare la query di supporto")

    *Individuare la query di supporto*
4. Sostituire il valore dell'attributo larghezza massima impostato in 850 px con **10px**, per disabilitare il rendering e premere **CTRL + S** per salvare le modifiche. Tornare al browser e premere **CTRL + F5** per aggiornare la pagina con le modifiche apportate. Notare le differenze in entrambe le tabelle quando si modifica la larghezza della finestra.

    ![In a sinistra, applica la pagina di @media stile, lo stile di destra viene omesso](whats-new-in-aspnet-mvc-4/_static/image17.png "In a sinistra, applica la pagina il @media stile, lo stile di destra viene omesso")

    *Nella parte sinistra, applica la pagina di @media stile, lo stile di destra viene omesso*

    A questo punto, analizziamo cosa accade nei dispositivi mobili:

    ![In a sinistra, applica la pagina di @media stile, lo stile di destra viene omesso](whats-new-in-aspnet-mvc-4/_static/image18.png "In a sinistra, applica la pagina il @media stile, lo stile di destra viene omesso")

    *Nella parte sinistra, applica la pagina di @media stile, lo stile di destra viene omesso*

    Sebbene si noterà che le modifiche quando viene eseguito il rendering della pagina in un Web browser non sono molto importanti quando si utilizza un dispositivo mobile diventano più evidenti le differenze. Sul lato sinistro dell'immagine, si noterà che lo stile personalizzato migliorata la leggibilità.

    Il rendering adattivo è utilizzabile in più scenari, rendendo più semplice applicare condizionale stile a un sito Web e risolvere problemi comuni di stile con un approccio semplice.

    Le query di supporto CSS e tag meta Viewport non sono specifiche di ASP.NET MVC 4, in modo da poter sfruttare queste funzionalità in qualsiasi applicazione web.
5. In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Esercizio 2: Creare l'applicazione Web Raccolta foto

In questo esercizio, verrà utilizzata in un'applicazione Raccolta foto per visualizzare le foto. Si inizierà con il modello di progetto ASP.NET MVC 4 e quindi si aggiungerà una funzionalità per recuperare le foto da un servizio e li visualizzano nella home page.

Nell'esercizio seguente, si aggiornerà questa soluzione per migliorare il modo in cui che viene visualizzato nei dispositivi mobili.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Attività 1 - Creazione di un servizio di foto di simulazione

In questa attività si creerà una simulazione del servizio per recuperare il contenuto che verrà visualizzato nella raccolta foto. A tale scopo, si aggiungerà un nuovo controller che restituirà semplicemente un file JSON con i dati di ogni foto.

1. Aprire **Visual Studio** se non è già aperto.
2. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere **applicazione Web ASP.NET MVC 4.** Denominare il progetto **PhotoGallery**, selezionare un percorso (o lasciare il valore predefinito) e fare clic su **OK**. In alternativa, è possibile continuare a lavorare da ASP.NET MVC 4 esistente **applicazione Internet** soluzione da **esercizio 1** e ignorare il passaggio successivo.
3. Nel **nuovo progetto ASP.NET MVC 4** la finestra di dialogo, seleziona il **applicazione Internet** modello di progetto e fare clic su **OK**. Verificare che sia selezionato come il motore di visualizzazione Razor.
4. Nel **Esplora**, fare doppio clic su di **App\_dati** cartella del progetto e selezionare **Aggiungi | Elemento esistente**. Individuare il **Source\Assets\App\_dati** cartella di questa esercitazione e aggiungere il **Photos.json** file.
5. Creare un nuovo controller con il nome **PhotoController**. A tale scopo, fare clic su di **controller** cartella, passa a **Aggiungi** e selezionare **Controller.** Completare il nome del controller, lasciare il **controller MVC vuoto** modello e fare clic su **Aggiungi**.

    ![Aggiunta di PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "aggiungendo il PhotoController")

    *Aggiunta di PhotoController*
6. Sostituire il **indice** (metodo) con il codice seguente **raccolta** azione e restituire il contenuto del file JSON recente sono stati aggiunti al progetto.

    (- Frammento di codice *ASP.NET MVC 4 Lab - Ex02 - raccolta azione*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Premere **F5** per eseguire la soluzione e quindi passare all'URL seguente per testare il servizio di foto fittizie: `http://localhost:[port]/photo/gallery` (il valore [porta] dipende da porta corrente in cui è stata avviata l'applicazione). La richiesta a questo URL deve recuperare il contenuto del **Photos.json** file.

    ![Test del servizio di foto simulati](whats-new-in-aspnet-mvc-4/_static/image20.png "test del servizio di foto simulati")

    *Test del servizio di foto simulati*

In un'implementazione reale è possibile utilizzare [ASP.NET Web API](../../../../web-api/index.md) per implementare il servizio di raccolta foto. API Web ASP.NET è un framework che rende più semplice compilare servizi HTTP in grado di raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è la piattaforma ideale per compilare applicazioni RESTful in .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Attività 2: visualizzazione della raccolta foto

In questa attività si aggiornerà la Home page per visualizzare la raccolta foto tramite il servizio simulato creato nella prima attività di questo esercizio. Si aggiungere file di modello e aggiornare le visualizzazioni di raccolta.

1. In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.
2. Creare il **foto** classe il **modelli** cartella. A tale scopo, fare clic su di **modelli** cartella, selezionare **Aggiungi** e fare clic su **classe**. Quindi, impostare il nome su **Photo.cs** e fare clic su **Aggiungi**.
3. Aggiungere i membri seguenti per il **foto** classe.

    (- Frammento di codice *modello ASP.NET MVC 4 Lab - Ex02 - foto*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Aprire il file **HomeController.cs** dalla cartella **Controller**.
5. Aggiungere le istruzioni using seguenti.

    (- Frammento di codice *ASP.NET MVC 4 Lab - Ex02 - HomeController using*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Aggiornamento di **indice** azione da utilizzare **HttpClient** per recuperare i dati di raccolta e quindi utilizzare il **JavaScriptSerializer** di deserializzazione al modello di visualizzazione.

    (- Frammento di codice *operazione sull'indice di ASP.NET MVC 4 Lab - Ex02 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Aprire il **cshtml** file si trova sotto il **Views\Home** cartella e sostituire tutto il contenuto con il codice seguente.

    Questo codice consente di scorrere tutte le foto recuperate dal servizio e li visualizza in un elenco non ordinato.

    (- Frammento di codice *elenco foto di ASP.NET MVC 4 Lab - Ex02 -*)


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. Nel **Esplora**, fare doppio clic su di **contenuto** cartella del progetto e selezionare **Aggiungi | Elemento esistente**. Individuare il **Source\Assets\Content** cartella di questa esercitazione e aggiungere il **Site** file. È necessario confermare la sostituzione. Se si dispone di **Site.css** il file è aperto, è necessario per ricaricare il file anche confermare.
9. Aprire Esplora File e copiare l'intero **foto** cartella si trova sotto il **Source\Assets** cartella di questo laboratorio nella cartella radice del progetto in Esplora soluzioni.
10. Eseguire l'applicazione. Viene visualizzato la home page, visualizzare le foto nella raccolta.

    ![Raccolta foto](whats-new-in-aspnet-mvc-4/_static/image21.png "raccolta foto")

    *Raccolta foto*
11. In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Esercizio 3: Aggiunta del supporto per i dispositivi mobili

Uno degli aggiornamenti di chiave in ASP.NET MVC 4 è il supporto per lo sviluppo per dispositivi mobili. In questo esercizio verranno esplorati nuove funzionalità di ASP.NET MVC 4 per applicazioni per dispositivi mobili estendendo la soluzione PhotoGallery creato nell'esercizio precedente.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Attività 1: installazione jQuery Mobile in un'applicazione ASP.NET MVC 4

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex3-MobileSupport/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **Package Manager Console** facendo il **strumenti** &gt; **Gestione pacchetti libreria** &gt; **Package Manager Console** opzione di menu.

    ![Aprire la Console di gestione pacchetti NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "aprire la Console di gestione pacchetti NuGet")

    *Aprire la Console di gestione pacchetti NuGet*
3. Nella Console di gestione pacchetti, eseguire il comando seguente per installare il **jQuery.Mobile.MVC** pacchetto.

    jQuery Mobile è una libreria open source per la compilazione di interfaccia utente web ottimizzata per il tocco. Il pacchetto NuGet jQuery.Mobile.MVC include gli helper per l'utilizzo di jQuery Mobile con un'applicazione ASP.NET MVC 4.

    > [!NOTE]
    > Eseguendo il comando seguente, si verrà download della libreria di jQuery.Mobile.MVC da Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Questo comando Installa jQuery Mobile e alcuni file di supporto, inclusi i seguenti:

    - **Views/Shared/\_Layout.Mobile.cshtml**: è un jQuery Mobile layout ottimizzato per uno schermo piccolo. Quando il sito Web riceve una richiesta da un browser per dispositivi mobili, che sostituirà il layout originale (\_cshtml) con questo.
    - Un componente di selezione di visualizzazione: costituito il **Views/Shared/\_ViewSwitcher.cshtml** visualizzazione parziale e **ViewSwitcherController.cs** controller. Questo componente visualizzerà un collegamento nel browser per dispositivi mobili per consentire agli utenti di passare alla versione desktop della pagina.  
        ![Progetto raccolta foto con supporto mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "progetto raccolta foto con supporto per dispositivi mobili")

        *Progetto di raccolta foto con supporto per dispositivi mobili*
4. Registrare i dispositivi mobili bundle. A tale scopo, aprire il **Global.asax.cs** file e aggiungere la riga seguente.

    (- Frammento di codice *ASP.NET MVC 4 Lab - Ex03 - registro mobili bundle*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Eseguire l'applicazione utilizzando un web browser desktop.
6. Aprire il **emulatore di Windows Phone 7,** nella **Menu Start | Tutti i programmi | Windows Phone SDK 7.1 | Emulatore Windows Phone.**
7. Nella schermata start telefono, aprire Internet Explorer. Estrarre l'URL in cui avviare l'applicazione e passare a tale URL con il browser phone (ad esempio `http://localhost:[PortNumber]/`).

    Si noterà che l'applicazione avrà un aspetto diverso nell'emulatore Windows Phone, come il jQuery.Mobile.MVC ha creato nuove risorse nel progetto che mostrano viste ottimizzate per i dispositivi mobili.

    Si noti il messaggio nella parte superiore del telefono, che mostra il collegamento che consente di passare alla visualizzazione Desktop. Inoltre, il  **\_Layout.Mobile.cshtml** layout in cui è stato creato il pacchetto è stato installato è incluso un layout diverso nell'applicazione.

    > [!NOTE]
    > Finora, non sussiste alcun collegamento per tornare alla visualizzazione mobile. Per essere inclusa nelle versioni successive.

    ![Vista mobile della pagina principale della raccolta foto](whats-new-in-aspnet-mvc-4/_static/image24.png "mobili visualizzazione della pagina Home raccolta foto")

    *Vista mobile della pagina principale della raccolta foto*
8. In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Attività 2: creazione di visualizzazioni per dispositivi mobili

In questa attività si creerà una versione per dispositivi mobili della visualizzazione dell'indice con contenuto adattato per una migliore appareance nei dispositivi mobili.

1. Copia il **Views\Home\Index.cshtml** consente di visualizzare e incollarlo per creare una copia, rinominare il nuovo file **Index.Mobile.cshtml**.
2. Aprire il nuovo creata **Index.Mobile.cshtml** consente di visualizzare e sostituire &lt;ul&gt; tag con questo codice. In questo modo, si aggiornano i &lt;ul&gt; tag con le annotazioni dei dati per dispositivi mobili jQuery per usare i temi di jQuery mobili.


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Si noti che:
    > 
    > - Il **ruolo dati** attributo impostato su **listview** verrà eseguito il rendering di elenco con gli stili di listview.
    > 
    > - Il **dati inset** attributo impostato su true visualizzerà l'elenco con bordo arrotondato e margine.
    > 
    > - Il **filtro dati** attributo impostato su **true** genererà una casella di ricerca.
    > 
    > Altre informazioni sulle convenzioni per jQuery Mobile nella documentazione di progetto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Premere **CTRL + S** per salvare le modifiche.
4. Passare il **emulatore Windows Phone** e aggiornare il sito. Si noti il nuovo aspetto dell'elenco della raccolta, nonché la nuova casella di ricerca si trova nella parte superiore. Quindi, digitare una parola nella casella di ricerca (ad esempio, **Tulips**) per avviare una ricerca nella raccolta foto.

    ![Raccolta con lo stile di listview con filtro](whats-new-in-aspnet-mvc-4/_static/image25.png "utilizzando lo stile di listview con filtri di raccolta")

    *Raccolta con lo stile di listview con filtro*

    Per riepilogare, la Guida per la visualizzazione Mobilizer hanno utilizzato per creare una copia della visualizzazione dell'indice con il &quot;mobili&quot; suffisso. Questo suffisso indica ad ASP.NET MVC 4 che ogni richiesta generata da un dispositivo mobile utilizzerà questa copia dell'indice. Inoltre, è stato aggiornato la versione per dispositivi mobili della visualizzazione dell'indice da utilizzare jQuery Mobile per migliorare l'aspetto del sito nei dispositivi mobili.
5. Tornare a Visual Studio e aprire **Site.Mobile.css** sotto il **contenuto** cartella.
6. Correggere la posizione del titolo della foto per renderlo Mostra sul lato destro dell'immagine. A tale scopo, aggiungere il codice seguente per il **Site.Mobile.css** file.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Premere **CTRL + S** per salvare le modifiche.
8. Tornare al **emulatore Windows Phone** e aggiornare il sito. Si noti che il titolo di foto viene correttamente posizionato ora.

    ![Titolo posizionato sul lato destro dell'immagine](whats-new-in-aspnet-mvc-4/_static/image26.png "titolo posizionato sul lato destro dell'immagine")

    *Titolo posizionato sul lato destro dell'immagine*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Attività 3 - jQuery Mobile temi

Ogni layout e i widget di jQuery Mobile viene progettato attorno a un nuovo framework di CSS orientata agli oggetti che consente di applicare un tema completo unificati visual a siti e applicazioni.

Per impostazione predefinita jQuery Mobile tema include 5 campioni forniti lettere (, b, c, d, e) di riferimento rapido.

In questa attività, è possibile aggiornare il layout per dispositivi mobili per l'utilizzo di un tema diverso da quello predefinito.

1. Tornare a Visual Studio.
2. Aprire il  **\_Layout.Mobile.cshtml** file si trova in **Views\Shared**.
3. Trovare l'elemento div con il ruolo di dati impostato su &quot;pagina&quot; e aggiornare il **dati tema** a &quot; **e**&quot;.


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Premere **CTRL + S** per salvare le modifiche.
5. Aggiornare il sito nel **emulatore Windows Phone** e notare che la nuova combinazione di colori.

    ![Layout per dispositivi mobili con una combinazione di colori diversi](whats-new-in-aspnet-mvc-4/_static/image27.png "layout per dispositivi mobili con una combinazione di colori diversi")

    *Layout per dispositivi mobili con una combinazione di colori diversi*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Attività 4: utilizzo del componente di selezione di visualizzazione e il Browser si esegue l'override di funzioni

Una convenzione per dispositivi mobili con ottimizzazione per le pagine web consiste nell'aggiungere un collegamento il cui testo è un elemento come visualizzazione Desktop o modalità completa del sito che consente agli utenti di passare a una versione desktop della pagina. Il pacchetto jQuery.Mobile.MVC è incluso un esempio **-Selezione tipo di visualizzazione** componente per questo scopo utilizzato nel  **\_Layout.Mobile.cshtml** visualizzazione.

![Collegamento per passare alla visualizzazione Desktop](whats-new-in-aspnet-mvc-4/_static/image28.png "collegamento per passare alla visualizzazione Desktop")

*Collegamento per passare alla visualizzazione Desktop*

La selezione della vista utilizza una nuova funzionalità denominata **si esegue l'override di Browser**. Questa funzionalità consente di considerare le richieste come provenienti da un altro browser (agente utente) rispetto a quella da che effettivamente provenire.

In questa attività si esploreranno l'implementazione di esempio di una selezione di visualizzazione aggiunta jQuery.Mobile.MVC e il nuovo browser si esegue l'override delle funzionalità in ASP.NET MVC 4.

1. Tornare a Visual Studio.
2. Aprire il  **\_Layout.Mobile.cshtml** visualizzazione si trova sotto il **Views\Shared** cartella e notare il componente di selezione di visualizzazione a cui fa riferimento come visualizzazione parziale.

    ![Layout per dispositivi mobili utilizzando il componente di visualizzazione selezione](whats-new-in-aspnet-mvc-4/_static/image29.png "layout per dispositivi mobili utilizzando il componente di selezione di visualizzazione")

    *Layout per dispositivi mobili utilizzando il componente di selezione di visualizzazione*
3. Aprire il  **\_ViewSwitcher.cshtml** visualizzazione parziale.

    La visualizzazione parziale viene utilizzato il nuovo metodo **ViewContext.HttpContext.GetOverriddenBrowser()** per determinare l'origine della richiesta web e visualizzare il collegamento corrispondente per passare uno alle visualizzazioni dei Desktop o Mobile.

    Il **GetOverridenBrowser** metodo restituisce un **HttpBrowserCapabilitiesBase** istanza che corrisponde all'agente utente attualmente impostata per la richiesta (effettivo o sottoposto a override). È possibile utilizzare questo valore per ottenere le proprietà, ad esempio **IsMobileDevice**.

    ![Visualizzazione parziale ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "visualizzazione parziale ViewSwitcher")

    *Visualizzazione parziale ViewSwitcher*
4. Aprire il **ViewSwitcherController.cs** classe si trova nel **controller** cartella. Check-out di tale azione SwitchView viene chiamato dal collegamento nel componente ViewSwitcher e notare i nuovi metodi HttpContext.

    - Il **HttpContext.ClearOverridenBrowser()** metodo rimuove qualsiasi agente utente sottoposto a override per la richiesta corrente.
    - Il **HttpContext.SetOverridenBrowser()** metodo esegue l'override di valore dell'agente utente effettivo della richiesta utilizzando l'agente utente specificato.  
        ![Controller ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")  
*Controller ViewSwitcher*

        Sostituzione di browser è una funzionalità centrale di ASP.NET MVC 4, anch ' esso disponibile anche se non si installa il pacchetto jQuery.Mobile.MVC. Tuttavia, questa funzionalità riguarda solo visualizzazione, layout e visualizzazione parziale e non si applica una delle funzionalità che dipendono dall'oggetto Request.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Attività 5 - aggiunta al commutatore della visualizzazione nella vista del Desktop

In questa attività, è possibile aggiornare il layout desktop per includere la selezione di visualizzazione. In questo modo gli utenti mobili tornare alla visualizzazione mobile durante l'esplorazione visualizzazione desktop.

1. Aggiornare il sito nel **emulatore Windows Phone**.
2. Fare clic su di **visualizzazione Desktop** collegamento nella parte superiore della raccolta. Si noti non esiste alcun commutatore della visualizzazione nella visualizzazione desktop consente che tornare alla visualizzazione mobile.
3. Tornare a Visual Studio e aprire il  **\_cshtml** visualizzazione.
4. Individuare la sezione di accesso e inserire una chiamata per eseguire il rendering di  **\_ViewSwitcher** visualizzazione parziale riportato di seguito il  **\_LogOnPartial** visualizzazione parziale. Premere quindi **CTRL + S** per salvare le modifiche.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Premere **CTRL + S** per salvare le modifiche.
6. Aggiornare la pagina nell'emulatore Windows Phone e fare doppio clic su schermo per fare zoom avanti. Si noti che verrà visualizzata la home page di **vista Mobile** collegamento che consente di passare alla visualizzazione desktop da dispositivi mobili.

    ![Visualizzare il rendering nella visualizzazione desktop selezione](whats-new-in-aspnet-mvc-4/_static/image32.png "Selezione tipo di visualizzazione eseguito il rendering nella visualizzazione desktop")

    *Selezione tipo di visualizzazione eseguito il rendering nella visualizzazione desktop*
7. Passare alla visualizzazione Mobile nuovamente e passare a **su** pagina (http://localhost [porta] / Home/su). Si noti che, anche se è stata creata una vista About.Mobile.cshtml, informazioni su verrà visualizzata la pagina utilizzando il layout di dispositivi mobili (\_Layout.Mobile.cshtml).

    ![Informazioni sulla pagina](whats-new-in-aspnet-mvc-4/_static/image33.png "sulla pagina")

    *Informazioni sulla pagina*
8. Infine, è possibile aprire il sito in un browser Web per computer desktop. Si noti che nessuno dei precedenti aggiornamenti riguarda la visualizzazione desktop.

    ![Visualizzazione desktop PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "visualizzazione desktop PhotoGallery")

    *Visualizzazione desktop PhotoGallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Attività 6: creazione di nuove modalità di visualizzazione

La nuova funzionalità di modalità di visualizzazione consente a un'applicazione di selezionare le viste in base al browser che genera la richiesta. Ad esempio, un browser desktop richiede la Home page, l'applicazione verrà restituito il **Views\Home\Index.cshtml** modello. Quindi, se un browser per dispositivi mobili richiede la Home page, l'applicazione verrà restituito il **Views\Home\Index.mobile.cshtml** modello.

In questa attività si creerà un layout personalizzato per i dispositivi iPhone e sarà necessario simulare le richieste provenienti da dispositivi iPhone. A tale scopo, è possibile utilizzare entrambi un emulatore o simulatore di iPhone (ad esempio [Electric simulatore di dispositivi mobili](http://www.electricplum.com/)) o un browser con i componenti aggiuntivi che modificano l'agente utente. Per istruzioni su come impostare la stringa agente utente in un browser Safari emulare un iPhone, vedere [descritto come consentire Safari fingono è IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) nel blog di David Alison.

**Si noti che questa attività è facoltativa ed è possibile continuare del laboratorio senza eseguirla.**

1. In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.
2. Aprire **Global.asax.cs** e aggiungere la seguente istruzione using.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Aggiungere il codice evidenziato di seguito all'applicazione\_Start (metodo).

    (- Frammento di codice *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    È stato registrato un nuovo **DefaultDisplayMode denominato &quot;iPhone&quot;**, all'interno statico **DisplayModeProvider.Instance.Modes** elenco statico, che verrà confrontato con ogni richiesta in ingresso. Se la richiesta in ingresso contiene la stringa &quot;iPhone&quot;, ASP.NET MVC troverà le visualizzazioni il cui nome contiene il &quot;iPhone&quot; suffisso. Il parametro 0 indica specifica è la nuova modalità; ad esempio, questa vista è più specifica generale &quot;.mobile&quot; regola corrispondente richieste dai dispositivi mobili.

    Dopo questo codice viene eseguito quando un browser iPhone che genera una richiesta, l'applicazione utilizzerà il **Views\Shared\\_Layout.iPhone.cshtml** layout verrà creato nei passaggi successivi.

    > [!NOTE]
    > In questo modo, il test della richiesta per iPhone è stato semplificato per scopi dimostrativi e potrebbe non funzionare come previsto per ogni stringa agente utente di iPhone (per il test di esempio viene fatta distinzione tra maiuscole e minuscole).
4. Creare una copia del  **\_Layout.Mobile.cshtml** file nel **Views\Shared** cartella e rinominare la copia &quot;  **\_Layout.iPhone.csthml** &quot;.
5. Aprire  **\_Layout.iPhone.csthml** creato nel passaggio precedente.
6. Trovare l'elemento div con l'attributo data-role impostato su **pagina** e modificare il **dati tema** attributo &quot; **un**&quot;.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    Ora si dispone di 3 layout nell'applicazione ASP.NET MVC 4:

    1. **\_Cshtml**: layout predefinito utilizzato per i browser desktop.
    2. **\_Layout.Mobile.cshtml**: layout predefinito utilizzato per i dispositivi mobili.
    3. **\_Layout.iPhone.cshtml**: layout specifico per i dispositivi iPhone, utilizzando una combinazione di colori diversi per distinguere da \_Layout.mobile.cshtml.
7. Premere **F5** per eseguire l'applicazione e selezionare il sito di **emulatore Windows Phone**.
8. Aprire un **simulatore di iPhone** (vedere [appendice C](#AppendixC) per istruzioni su come installare e configurare un simulatore di iPhone) e passare al sito troppo. Si noti che ogni phone utilizza il modello specifico.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Utilizzo di visualizzazioni diverse per ogni dispositivo mobile*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Esercizio 4: Utilizzo di controller asincroni

Microsoft .NET Framework 4.5 introduce nuove funzionalità del linguaggio in c# e Visual Basic per fornire una nuova base per la modalità asincrona nella programmazione .NET. Questo nuovo foundation rende la programmazione asincrona simile a - e circa semplice come quello - programmazione sincrona. Questo punto si è in grado di scrivere metodi di azione asincroni in ASP.NET MVC 4 con la **AsyncController** classe. È possibile utilizzare i metodi di azione asincroni per l'esecuzione prolungata non associate alla CPU richieste. Ciò evita che il server Web di esecuzione del lavoro durante l'elaborazione della richiesta di blocco. La classe AsyncController viene in genere utilizzata per le chiamate al servizio Web con esecuzione prolungata.

Questo esercizio illustra i concetti fondamentali dell'operazione asincrona in ASP.NET MVC 4. Se si desidera un approfondimento, è possibile leggere l'articolo seguente: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Attività 1: implementazione di un Controller asincrono

1. Aprire il **iniziare** soluzione che si trova in **origine/Ex4-Async/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **HomeController.cs** classe il **controller** cartella.
3. Aggiungere la seguente istruzione using.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Aggiornamento di **HomeController** classe da cui ereditare **AsyncController**. I controller che derivano da AsyncController consentono ad ASP.NET di elaborare le richieste asincrone e possono comunque metodi di azione sincroni di servizio.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Aggiungere il **async** (parola chiave) per il **indice** (metodo) e il tipo restituito **attività&lt;ActionResult&gt;**.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Il **async** (parola chiave) è uno delle nuove parole chiave fornisce .NET Framework 4.5; indica al compilatore che questo metodo contiene codice asincrono. Oggetto **attività** oggetto rappresenta un'operazione asincrona che può essere completata in seguito a un certo punto.
6. Sostituire il **client. GetAsync()** chiamata con la versione completa async utilizzando parola chiave await come illustrato di seguito.

    (- Frammento di codice *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Nella versione precedente, si utilizzava il **risultato** proprietà il **attività** oggetto per bloccare il thread fino a quando il risultato viene restituito (versione di sincronizzazione).
    > 
    > Aggiunta di **await** parola chiave indica al compilatore di attendere in modo asincrono l'attività restituita dalla chiamata al metodo. Ciò significa che il resto del codice verrà eseguito come un callback solo dopo il metodo atteso il completamento. Un altro aspetto da notare è che non è necessario modificare il blocco try-catch per ottenere questo risultato: le eccezioni che si verificano in background o in primo piano continuano a essere intercettate senza necessità di operazioni utilizzando un gestore fornito dal framework.
7. Modificare il codice per continuare con l'implementazione asincrona, sostituendo le righe con il nuovo codice, come illustrato di seguito

    (- Frammento di codice *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Eseguire l'applicazione. Non si noterà che nessuna modifica rilevante, ma il codice non verrà bloccata un thread dal pool di thread, un migliore utilizzo delle risorse del server e di migliorare le prestazioni.

    > [!NOTE]
    > Maggiori informazioni sulle nuove funzionalità di programmazione asincrona nell'ambiente lab &quot; **la programmazione asincrona in .NET 4.5 con c# e Visual Basic** &quot; inclusi nel Kit di formazione Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Attività 2: i timeout di gestione con i token di annullamento

Metodi di azione asincroni che restituiscono istanze di attività possono inoltre supportare i timeout. In questa attività si aggiornerà il codice del metodo di indice per la gestione di uno scenario di timeout usando un token di annullamento.

1. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.
2. Aggiungere la seguente istruzione using il **HomeController.cs** file.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Aggiornare l'operazione sull'indice per ricevere un **CancellationToken** argomento.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Aggiornamento di **GetAsync** chiamata per passare il token di annullamento.

    (- Frammento di codice *SendAsync Lab - Ex04 - ASP.NET MVC 4 con oggetto CancellationToken*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decorare il *indice* metodo con un **AsyncTimeout** attributo impostato su 500 millisecondi e **HandleError** attributo configurato per gestire  **TaskCanceledException** mediante il reindirizzamento a un **TimedOut** visualizzazione.

    (- Frammento di codice *gli attributi di ASP.NET MVC 4 Lab - Ex04 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Aprire il **PhotoController** classe e l'aggiornamento di **raccolta** metodo ritardare l'esecuzione 1000 millisecondi (1 secondo), per simulare un'attività a esecuzione prolungata.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Aprire il **Web. config** file e abilitare gli errori personalizzati, aggiungere l'elemento seguente.


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Creare una nuova vista in **Views\Shared** denominato **TimedOut** e usare il layout predefinito. In Esplora soluzioni fare doppio clic su di **Views\Shared** cartella e selezionare **Aggiungi | Visualizzazione**.

    ![Utilizzo di visualizzazioni diverse per ogni dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image36.png "usando diverse visualizzazioni diverse per ogni dispositivo mobile")

    *Utilizzo di visualizzazioni diverse per ogni dispositivo mobile*
9. Aggiornamento di **TimedOut** visualizzare il contenuto, come illustrato di seguito.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Eseguire l'applicazione e passare all'URL radice. Come è stato aggiunto un **Sleep** di 1000 millisecondi, si otterrà un errore di timeout, generato dal **AsyncTimeout** degli attributi e intercettare per il **HandleError** attributo.

    ![Eccezione di timeout gestita](whats-new-in-aspnet-mvc-4/_static/image37.png "gestita l'eccezione di timeout")

    *Eccezione di timeout gestita*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice d: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questa esercitazione pratica, è stata osservata alcune delle nuove funzionalità residenti in ASP.NET MVC 4. Sono stati trattati i concetti seguenti:

- Sfruttare i miglioramenti per i modelli, compresi progetto di MVC ASP.NET il nuovo modello di progetto di applicazione per dispositivi mobili
- Utilizzare le query di supporto CSS e attributo viewport HTML5 per migliorare la visualizzazione nei dispositivi mobili
- Utilizzare jQuery Mobile per l'ottimizzazione progressivi e per la compilazione di interfaccia utente web ottimizzata per il tocco
- Creare visualizzazioni specifiche di dispositivi mobili
- Utilizzare il componente di selezione di visualizzazione per passare tra le visualizzazioni di dispositivi mobili e desktop nell'applicazione
- Creare i controller asincroni utilizzando il supporto delle attività

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Appendice a: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](whats-new-in-aspnet-mvc-4/_static/image38.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](whats-new-in-aspnet-mvc-4/_static/image39.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](whats-new-in-aspnet-mvc-4/_static/image40.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](whats-new-in-aspnet-mvc-4/_static/image41.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)***

1. Fare clic in cui si desidera inserire il frammento di codice.
2. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
3. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](whats-new-in-aspnet-mvc-4/_static/image42.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](whats-new-in-aspnet-mvc-4/_static/image43.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Appendice b: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

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
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express per il riquadro Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Appendice c: installazione WebMatrix 2 e iPhone simulatore

Per eseguire il sito in un dispositivo simulato iPhone è possibile utilizzare l'estensione di WebMatrix &quot;Electric simulatore di dispositivi mobili per iPhone&quot;. Inoltre, è possibile configurare la stessa estensione per eseguire il simulatore di Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Attività 1: installazione di WebMatrix 2

1. Passare a [ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *WebMatrix 2*&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

    ![Installare WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "installare WebMatrix 2")

    *Installare WebMatrix 2*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](whats-new-in-aspnet-mvc-4/_static/image50.png "accettando le condizioni di licenza")

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Lo stato dell'installazione](whats-new-in-aspnet-mvc-4/_static/image51.png "l'avanzamento dell'installazione")

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-aspnet-mvc-4/_static/image52.png "installazione completata")

    *Installazione completata*
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Attività 2: installare l'estensione simulatore di iPhone

1. Eseguire **WebMatrix** e aprire i siti Web esistenti o crearne uno nuovo.
2. Fare clic su di **eseguire** pulsante il **Home** della barra multifunzione e selezionare **Aggiungi nuovo**.

    ![Aggiunta di nuova estensione di WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "aggiunta nuova estensione di WebMatrix")

    *Aggiunta di nuova estensione di WebMatrix*
3. Selezionare **iPhone simulatore** e fare clic su **installare**.

    ![Esplorazione delle estensioni di WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "estensioni esplorazione WebMatrix")

    *Esplorazione delle estensioni di WebMatrix*
4. I dettagli del pacchetto, fare clic su **installare** per continuare l'installazione dell'estensione.

    ![iPhone estensione simulatore](whats-new-in-aspnet-mvc-4/_static/image55.png "estensione simulatore di iPhone")

    *estensione simulatore di iPhone*
5. Leggere e accettare il contratto di licenza di estensione.

    ![Estensione di WebMatrix EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "con l'estensione di WebMatrix")

    *Con l'estensione di WebMatrix*
6. A questo punto, è possibile eseguire il sito Web da WebMatrix utilizzando l'opzione simulatore di iPhone.

    ![Eseguire utilizzando iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "eseguiti utilizzando iPhone")

    *Eseguire utilizzando iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Attività 3: configurazione di Visual Studio 2012 per eseguire un simulatore di iPhone

1. Aprire **Visual Studio 2012** e aprire qualsiasi sito Web o creare un nuovo progetto.
2. Fare clic sulla freccia rivolta verso il basso sul pulsante Esegui e selezionare **Sfoglia con**.

    ![Sfoglia con](whats-new-in-aspnet-mvc-4/_static/image58.png "Sfoglia con")

    *Sfoglia con*
3. Nel &quot;Esplora con&quot; finestra di dialogo, fare clic su **Aggiungi**.
4. Nel &quot;Aggiungi programma&quot; finestra di dialogo, utilizzare i valori seguenti:

    - **Programma**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(aggiornare di conseguenza il percorso)*
    - **Argomenti**: &quot;1&quot;
    - **Nome descrittivo**: simulatore di iPhone

    ![Aggiungi programma](whats-new-in-aspnet-mvc-4/_static/image59.png "Aggiungi programma")

    *Aggiungi programma di esplorare con*
5. Fare clic su **OK** e chiudere le finestre di dialogo.
6. Si è ora in grado di eseguire le applicazioni Web nel simulatore di iPhone da Visual Studio 2012.

    ![Sfoglia con iPhone simulatore](whats-new-in-aspnet-mvc-4/_static/image60.png "Sfoglia con simulatore di iPhone")

    *Sfoglia con simulatore di iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice d: la pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal

1. Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico. È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "accedere al portale Windows Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-aspnet-mvc-4/_static/image62.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Selezionare quindi **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale. Non include i passaggi per configurare un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-aspnet-mvc-4/_static/image63.png "creando un nuovo sito Web utilizzando Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna. Verificare il funzionamento del nuovo sito Web.

    ![Esplorazione per il nuovo sito web](whats-new-in-aspnet-mvc-4/_static/image64.png "esplorazione per il nuovo sito web")

    *Esplorazione per il nuovo sito web*

    ![Sito Web in esecuzione](whats-new-in-aspnet-mvc-4/_static/image65.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione del sito web](whats-new-in-aspnet-mvc-4/_static/image66.png "apertura delle pagine di gestione del sito web")

    *Aprire le pagine di gestione del sito Web*
7. Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.

    ![Profilo di pubblicazione del sito web di download](whats-new-in-aspnet-mvc-4/_static/image67.png "profilo di pubblicazione del sito web di download")

    *Profilo di pubblicazione del sito Web di download*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file del profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image68.png "il salvataggio del profilo di pubblicazione")

    *Salvare il file del profilo di pubblicazione*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL. Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi. Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard del Server Database SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "Dashboard del Server Database SQL")

    *Dashboard del Server Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) pulsante.

    ![Aggiunta indirizzo IP del Client](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confermare le modifiche*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.

    ![La pubblicazione dell'applicazione](whats-new-in-aspnet-mvc-4/_static/image73.png "la pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image74.png "l'importazione del profilo di pubblicazione")

    *L'importazione di profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Una volta completata la convalida, fare clic su **Avanti**.

    > [!NOTE]
    > Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.

    ![Convalida della connessione](whats-new-in-aspnet-mvc-4/_static/image75.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).

    ![Configurazione della distribuzione Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

    - Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.
    - In **nome utente** digitare il nome di accesso di amministratore di server.
    - In **Password** digitare la password dell'account di accesso amministratore server.
    - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

    ![Configurazione di stringa di connessione di destinazione](whats-new-in-aspnet-mvc-4/_static/image77.png "configurazione stringa di connessione di destinazione")

    *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database fare clic su **Sì**.

    ![Creazione del database](whats-new-in-aspnet-mvc-4/_static/image78.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **anteprima** pagina, fare clic su **pubblica**.

    ![Pubblicare l'applicazione web](whats-new-in-aspnet-mvc-4/_static/image80.png "pubblicare l'applicazione web")

    *Pubblicare l'applicazione web*
9. Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Windows Azure*
