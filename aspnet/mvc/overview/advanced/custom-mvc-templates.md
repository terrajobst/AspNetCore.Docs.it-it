---
uid: mvc/overview/advanced/custom-mvc-templates
title: Il modello MVC personalizzato | Documenti Microsoft
author: joeloff
description: Creare un modello come estensione VSIX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034607"
---
<a name="custom-mvc-template"></a>Modello MVC personalizzato
====================
da [viene Eloff](https://github.com/joeloff)

La versione di MVC 3 Tools Update per Visual Studio 2010 è stato introdotto una procedura guidata progetto separato per i progetti MVC. La modifica è stata determinata da due fattori. In primo luogo, l'introduzione di nuovi modelli in MVC 3 e il supporto per i motori di visualizzazione aggiuntive, ad esempio Razor causare sovraffollamento la finestra di dialogo Nuovo progetto in Visual Studio. In secondo luogo, i clienti erano richiesta dei punti di estendibilità ed è la creazione guidata nuovo progetto MVC sarebbe concedere ci la possibilità di rispondere alle richieste.

Aggiunta di modelli personalizzati: un processo complesso per basato sull'utilizzo del Registro di sistema per rendere visibile per la creazione guidata progetto MVC nuovi modelli. L'autore di un nuovo modello doveva eseguirne il wrapping in un file MSI per garantire che le voci del Registro di sistema necessarie verrebbero create al momento dell'installazione. L'alternativa consiste nel creare un file ZIP contenente il modello è disponibile in modo che l'utente finale creare manualmente le voci del Registro di sistema.

Nessuno dei metodi menzionati in precedenza è ideale in modo si è deciso di sfruttare alcune dell'infrastruttura esistente fornita da [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) estensioni per renderne più semplice di creare, distribuire e installare i modelli MVC personalizzati a partire da MVC 4 per Visual Studio 2012. Alcuni dei vantaggi forniti da questo approccio sono:

- Un'estensione VSIX può contenere più modelli che supportano le diverse lingue (c# e Visual Basic) e più motori di visualizzazione (ASPX e Razor).
- Un'estensione VSIX possono essere destinati a più SKU di Visual Studio inclusi SKU di Express.
- Il [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilita l'estensione a un vasto pubblico di distribuzione.
- È possibile aggiornare le estensioni VSIX rendendo più semplice creare nuove correzioni e gli aggiornamenti per i modelli personalizzati.

## <a name="prerequisites"></a>Prerequisiti

- Gli utenti devono avere familiarità con la creazione di modelli di progetto, incluso il markup richiesto per il file vstemplate e così via.
- Gli utenti richiedono che Visual Studio Professional e versione successiva installato. Gli SKU di Express non supportano la creazione di progetti VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installato.

## <a name="example"></a>Esempio

Il primo passaggio consiste nel creare un nuovo progetto VSIX usando c# o Visual Basic. Selezionare **File > Nuovo progetto**, quindi fare clic su **estendibilità** nel riquadro sinistro e selezionare il **progetto VSIX**.

![Nuovo progetto](custom-mvc-templates/_static/image1.jpg)

Dopo aver creato il progetto, verrà aperta la finestra di progettazione del progetto VSIX.

![Metadati della finestra di progettazione del progetto](custom-mvc-templates/_static/image2.jpg)

La finestra di progettazione può essere utilizzato per modificare alcune delle proprietà generale dell'estensione che verrà visualizzato agli utenti quando Sfoglia le estensioni installate in Visual Studio o installare l'estensione (**strumenti > estensioni e aggiornamenti**). Dopo aver completato le informazioni generali fare clic su di **scheda destinazioni di installazione**.

![Destinazioni di installazione della finestra di progettazione di progetto](custom-mvc-templates/_static/image3.jpg)

Questa scheda è utilizzata per specificare la SKU di versioni di Visual Studio supportate dall'estensione. Selezionare la casella di controllo **questo VSIX è installato per tutti gli utenti** per consentire le installazioni per computer del pacchetto VSIX. Fare clic su di **New** pulsante a destra per aggiungere SKU aggiuntive, ad esempio Web Developer Express (VWD).

![Aggiungi nuova destinazione installazione](custom-mvc-templates/_static/image4.jpg)

Se si prevede di supportare tutte le SKU Professional e versioni successive (Professional, Premium e Ultimate) è sufficiente selezionare lo SKU minimo della famiglia, **Microsoft.VisualStudio.Pro**. Ricordarsi di salvare tutte le modifiche dopo aver completato le destinazioni di installazione.

![Destinazioni di installazione della finestra di progettazione di progetto](custom-mvc-templates/_static/image5.jpg)

Il **asset** scheda viene usata per aggiungere tutti i file di contenuto per il progetto VSIX. Poiché MVC richiede metadati personalizzati si desidera modificare il XML non elaborato del file manifesto VSIX anziché il **asset** scheda per aggiungere contenuto. Per iniziare, aggiungere il contenuto del modello per il progetto VSIX. È importante che la struttura della cartella e il contenuto rispecchia il layout del progetto. Nell'esempio seguente contiene quattro modelli di progetto che derivano dal modello di progetto MVC di base. Assicurarsi che tutti i file che costituiscono il modello di progetto (tutti gli elementi sotto la cartella ProjectTemplates) vengono aggiunti per il **contenuto** itemgroup nel progetto VSIX del progetto che contiene ogni elemento e il file di  **CopyToOutputDirectory** e **IncludeInVsix** metadati impostati come illustrato nell'esempio riportato di seguito.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

In caso contrario, l'IDE tenterà di compilare il contenuto del modello quando si compila il progetto VSIX e verrà visualizzato un errore. File di codice nei modelli contengono spesso speciali [parametri di modello](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) utilizzato da Visual Studio quando il modello di progetto viene creata un'istanza e pertanto non può essere compilato nell'IDE.

![Esplora soluzioni](custom-mvc-templates/_static/image6.jpg)

Chiudere la finestra di progettazione del progetto VSIX, quindi fare clic con il pulsante destro sul **source.extension.manifest** file in **Esplora** e selezionare **Apri con** e scegliere il **(XML Editor di testo)** opzione.

![Apri finestra di dialogo](custom-mvc-templates/_static/image7.jpg)

Creare un **&lt;asset&gt;** elemento e aggiungere un **&lt;Asset&gt;** elemento per ogni file che deve essere inclusi in VSIX. Il **tipo** attributo di ogni **&lt;Asset&gt;** elemento deve essere impostato su **Microsoft.VisualStudio.Mvc.Template**. Si tratta di uno spazio dei nomi personalizzato che riconosce solo la creazione guidata progetto MVC. Consultare la documentazione di Schema 2.0 VSIX per ulteriori informazioni sulla struttura e al layout del file manifesto.

Aggiungere solo i file per l'estensione VSIX non è sufficiente per registrare i modelli con la procedura guidata MVC. È necessario fornire informazioni quali il nome del modello, descrizione, motori di visualizzazione supportate e linguaggio di programmazione per la procedura guidata MVC. Queste informazioni sono portate in attributi personalizzati associati il **&lt;Asset&gt;** per ogni elemento **vstemplate** file.

&lt;Asset d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language =&quot;c#&quot;

ViewEngine =&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

Titolo =&quot;applicazione Web di base personalizzata&quot;

Descrizione =&quot;un modello personalizzato derivato da un'applicazione web MVC di base (Razor)&quot;

Versione =&quot;4.0&quot;/&gt;

Di seguito è una spiegazione di attributi personalizzati che devono essere presenti:

- **ProjectType** deve essere impostato su MVC.
- **Linguaggio** definisce il linguaggio di sviluppo supportato dal modello. I valori validi sono in c# o VB.
- **ViewEngine** designa il motore di visualizzazione supportato dal modello, ad esempio Razor o Aspx. È possibile specificare un valore personalizzato per questo campo.
- **TemplateId** viene utilizzato per raggruppare i modelli. Se il valore corrisponde a un ID di modello esistente sarà override modelli precedentemente registrati con la procedura guidata MVC.
- **Titolo** definisce la descrizione breve visualizzata nella procedura guidata MVC di sotto di ogni modello di progetto.
- **Descrizione** designa una descrizione più dettagliata del modello.

Dopo che tutti i file aggiunti al manifesto e salvato, si noterà che il **asset** scheda nella finestra di progettazione verrà visualizzati tutti i file, ma non gli attributi personalizzati aggiunti al **&lt;Asset&gt;** elementi per il **vstemplate** file.

![Finestra di progettazione risorse di progetto](custom-mvc-templates/_static/image8.jpg)

Ora è comunque per compilare il progetto VSIX e installarlo.

Assicurarsi che tutte le istanze di Visual Studio siano chiuse nel computer in cui si intende testare l'estensione VSIX. Visual Studio Cerca nuove estensioni durante l'avvio, pertanto se l'IDE è aperto durante l'installazione di un progetto VSIX è necessario riavviare Visual Studio. In Esplora, fare doppio clic sul file VSIX per avviare il **VSIX Installer**, fare clic su **installare** e quindi avviare Visual Studio.

![Programma di installazione VSIX](custom-mvc-templates/_static/image9.jpg)

Nel menu, selezionare **strumenti > estensioni e aggiornamenti** per confermare che l'estensione è stata installata. Se il programma di installazione di VSIX segnalati errori durante l'installazione dell'estensione è possibile visualizzare il log del programma di installazione di VSIX per altre informazioni. Il log viene creato in genere nel **% temp %** cartella dell'utente che ha installato l'estensione, ad esempio **C:\Users\Bob\AppData\Local\Temp**.

![Estensioni e aggiornamenti](custom-mvc-templates/_static/image10.jpg)

Dopo aver chiuso la finestra è possibile creare un progetto MVC 4 per vedere se i nuovi modelli vengono visualizzati nella procedura guidata MVC.

![Nuovo progetto ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitazioni

1. La procedura guidata MVC non supporta modelli personalizzati localizzati.
2. La procedura guidata non segnalerà gli errori se non è possibile individuare modelli personalizzati. Se uno qualsiasi degli attributi personalizzati necessari sono assente, il modello verrà semplicemente esclusi dalla procedura guidata.
