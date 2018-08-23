---
uid: mvc/overview/advanced/custom-mvc-templates
title: Modello MVC personalizzato | Microsoft Docs
author: joeloff
description: Creare un modello come estensione VSIX.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 3c14bc6feb144a52773bf7bd4c23df24966a9ebb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832804"
---
<a name="custom-mvc-template"></a>Modello MVC personalizzato
====================
da [Jacques Eloff](https://github.com/joeloff)

La versione di MVC 3 Tools Update per Visual Studio 2010 ha introdotto una procedura guidata progetto separato per i progetti MVC. La modifica è stata determinata da due fattori. In primo luogo, l'introduzione di nuovi modelli in MVC 3 e il supporto per i motori di visualizzazione aggiuntive, ad esempio Razor causare sovraffollamento la finestra di dialogo Nuovo progetto in Visual Studio. In secondo luogo, i clienti erano stata porre per i punti di estendibilità ed è la creazione guidata nuovo progetto MVC sarebbe concedere noi la possibilità di rispondere a queste richieste.

Aggiunta di modelli personalizzati, è stato un processo difficile e si basava sull'uso del Registro di sistema per rendere visibile per la creazione guidata progetto MVC nuovi modelli. L'autore di un nuovo modello era necessario eseguirne il wrapping in un file MSI per garantire che le voci del Registro di sistema necessarie verrebbe create al momento dell'installazione. L'alternativa consisteva nel rendere un file ZIP contenente il modello è disponibile in modo che l'utente finale creare manualmente le voci del Registro di sistema.

Nessuno degli approcci indicati in precedenza è ideale in modo che abbiamo deciso di sfruttare alcune dell'infrastruttura esistente fornita dal [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) estensioni per renderne più semplice per creare, distribuire e installare i modelli MVC personalizzati a partire da MVC 4 per Visual Studio 2012. Alcuni dei vantaggi forniti da questo approccio sono:

- Un'estensione VSIX può contenere più modelli che supportano le diverse lingue (c# e Visual Basic) e più motori di visualizzazione (ASPX e Razor).
- Un'estensione VSIX può far riferimento a più SKU di Visual Studio inclusi SKU di Express.
- Il [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilita la distribuzione l'estensione a un vasto pubblico.
- È possibile aggiornare le estensioni VSIX rendendo più semplice creare correzioni e aggiornamenti per i modelli personalizzati.

## <a name="prerequisites"></a>Prerequisiti

- Gli utenti devono avere familiarità con la creazione di modelli di progetto, incluso il markup necessario per il file vstemplate e così via.
- Gli utenti dovranno disporre di Visual Studio Professional e versioni successive. SKU di Express non supportano la creazione di progetti VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installato.

## <a name="example"></a>Esempio

Il primo passaggio consiste nel creare un nuovo progetto VSIX usando c# o Visual Basic. Selezionare **File > Nuovo progetto**, quindi fare clic su **Extensibility** nel riquadro a sinistra e selezionare il **progetto VSIX**.

![Nuovo progetto](custom-mvc-templates/_static/image1.jpg)

Dopo aver creato il progetto, verrà aperta la finestra di progettazione VSIX.

![Metadati della finestra di progettazione del progetto](custom-mvc-templates/_static/image2.jpg)

La finestra di progettazione può essere utilizzato per modificare alcune delle proprietà generale dell'estensione che verrà visualizzato agli utenti quando si installa l'estensione o Sfoglia le estensioni installate in Visual Studio (**strumenti > estensioni e aggiornamenti**). Dopo aver completato le informazioni generali fare clic sui **scheda destinazioni di installazione**.

![Destinazioni di installazione della finestra di progettazione di progetto](custom-mvc-templates/_static/image3.jpg)

Questa scheda consente di specificare la SKU e versioni di Visual Studio supportate dall'estensione. Selezionare la casella di controllo **VSIX questa viene installata per tutti gli utenti** per abilitare le installazioni per computer di VSIX. Fare clic sui **New** pulsante a destra per aggiungere gli SKU aggiuntivi, ad esempio Web Developer Express (VWD).

![Aggiungi nuova destinazione di installazione](custom-mvc-templates/_static/image4.jpg)

Se si prevede di supportare tutti gli SKU Professional e versioni successive (Professional, Premium e Ultimate) è sufficiente selezionare lo SKU minimo della famiglia **Microsoft.VisualStudio.Pro**. Ricordarsi di salvare tutte le modifiche dopo aver completato le destinazioni di installazione.

![Destinazioni di installazione della finestra di progettazione di progetto](custom-mvc-templates/_static/image5.jpg)

Il **asset** scheda viene usata per aggiungere tutti i file di contenuto per il progetto VSIX. Poiché MVC richiede metadati personalizzati si desidera modificare il XML non elaborato del file manifesto VSIX invece di usare la **asset** pressione di tab per aggiungere contenuto. Iniziare aggiungendo il contenuto del modello di progetto VSIX. È importante che la struttura della cartella e il contenuto rispecchia il layout del progetto. L'esempio seguente contiene quattro modelli di progetto che sono stati derivati dal modello di progetto MVC di base. Assicurarsi che tutti i file che costituiscono il modello di progetto (tutti gli elementi sotto la cartella ProjectTemplates) vengano aggiunti al **contenuti** itemgroup in VSIX del progetto file e che ogni elemento contiene il  **CopyToOutputDirectory** e **IncludeInVsix** metadati impostati come illustrato nell'esempio seguente.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

In caso contrario, verrà effettuato un tentativo dell'IDE compilare il contenuto del modello quando si compila il progetto VSIX e verrà visualizzato un errore. I file di codice nei modelli di spesso contengono speciali [parametri di modello](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) usate da Visual Studio quando il modello di progetto viene creata un'istanza e pertanto non può essere compilato nell'IDE.

![Esplora soluzioni](custom-mvc-templates/_static/image6.jpg)

Chiudere la finestra di progettazione VSIX, quindi fare clic con il pulsante destro sul **source.extension.manifest** del file in **Esplora soluzioni** e selezionare **Apri con** e scegliere il **(XML Editor di testo)** opzione.

![Apri finestra di dialogo](custom-mvc-templates/_static/image7.jpg)

Creare un **&lt;Assets&gt;** elemento e aggiungere un **&lt;Asset&gt;** (elemento) per ogni file che deve essere inclusi in VSIX. Il **tipo** attributo della ognuno **&lt;Asset&gt;** deve essere impostato su **Microsoft.VisualStudio.Mvc.Template**. Si tratta di uno spazio dei nomi personalizzato che riconosce solo la creazione guidata progetto MVC. Vedere la documentazione di Schema VSIX 2.0 per altre informazioni sulla struttura e layout del file manifesto.

Aggiungere solo i file per il progetto VSIX non è sufficiente registrare i modelli con la procedura guidata MVC. È necessario fornire informazioni quali il nome del modello, descrizione, i motori di visualizzazione supportate e linguaggio di programmazione per la procedura guidata MVC. Queste informazioni viene eseguite negli attributi personalizzati associati con la **&lt;Asset&gt;** per ogni elemento **vstemplate** file.

&lt;Asset d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language =&quot;c#&quot;

ViewEngine =&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

Titolo =&quot;applicazione Web di base personalizzata&quot;

Description =&quot;un modello personalizzato derivato da un'applicazione web MVC di base (Razor)&quot;

Versione =&quot;4.0&quot;/&gt;

Di seguito è una spiegazione degli attributi personalizzati che devono essere presenti:

- **Tipoprogetto** deve essere impostata su MVC.
- **Linguaggio** definisce il linguaggio di sviluppo supportato dal modello. I valori validi sono in c# o VB.
- **ViewEngine** designa il motore di visualizzazione supportato dal modello, ad esempio Razor o Aspx. È possibile specificare un valore personalizzato per questo campo.
- **TemplateId** viene impiegata per raggruppare i modelli. Se il valore corrisponde a un ID modello esistente sarà eseguire l'override modelli registrati in precedenza con la procedura guidata MVC.
- **Titolo** definisce la descrizione breve visualizzata nella procedura guidata MVC di sotto di ogni modello di progetto.
- **Descrizione** designa una descrizione più dettagliata del modello.

Dopo avere aggiunto tutti i file al manifesto e stato salvato, si noterà che il **asset** scheda nella finestra di progettazione verrà visualizzati tutti i file, ma non gli attributi personalizzati aggiunto in precedenza per il **&lt;Asset&gt;** gli elementi per il **vstemplate** file.

![Asset della finestra di progettazione del progetto](custom-mvc-templates/_static/image8.jpg)

A questo punto resta che compila il progetto VSIX e installarlo.

Assicurarsi che tutte le istanze di Visual Studio sono chiuse nel computer in cui si vuole testare l'estensione VSIX. Visual Studio analizza le nuove estensioni durante l'avvio, pertanto se l'IDE è aperto durante l'installazione di un progetto VSIX sarà necessario riavviare Visual Studio. In Explorer, fare doppio clic sul file VSIX per avviare il **programma di installazione VSIX**, fare clic su **installare** e quindi avviare Visual Studio.

![Programma di installazione VSIX](custom-mvc-templates/_static/image9.jpg)

Nel menu, selezionare **strumenti > estensioni e aggiornamenti** per confermare che è stata installata l'estensione. Se il programma di installazione VSIX ha segnalato errori durante l'installazione dell'estensione è possibile visualizzare il log del programma di installazione VSIX per altre informazioni. Il log viene in genere creato nel **% temp %** cartella dell'utente che ha installato l'estensione, ad esempio **C:\Users\Bob\AppData\Local\Temp**.

![Estensioni e aggiornamenti](custom-mvc-templates/_static/image10.jpg)

Dopo avere chiuso la finestra è possibile creare un progetto MVC 4 per vedere se i nuovi modelli vengono visualizzati nella procedura guidata MVC.

![Nuovo progetto ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitazioni

1. La procedura guidata MVC non supporta i modelli personalizzati localizzati.
2. La procedura guidata non segnalerà eventuali errori se non riesce a individuare i modelli personalizzati. Se uno qualsiasi degli attributi personalizzati necessari sono assente, il modello verrebbe semplicemente escluso dalla procedura guidata.
