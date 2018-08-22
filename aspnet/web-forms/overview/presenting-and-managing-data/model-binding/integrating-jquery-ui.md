---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: L'integrazione JQuery UI Datepicker con associazione di modelli e web form | Microsoft Docs
author: tfitzmac
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 81cc5f010f2c6c7707223679717e34108a5a7aa3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826877"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>L'integrazione JQuery UI Datepicker con associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come aggiungere la UI JQuery [widget Datepicker](http://jqueryui.com/datepicker/) in un Web Form, utilizzo del modello di associazione per aggiornare il database con il valore selezionato.
> 
> Questa esercitazione si basa sul progetto creato nel [primo](retrieving-data.md) e [secondo](updating-deleting-and-creating-data.md) parti della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

In questa esercitazione, sarà:

1. Aggiungere una proprietà nel modello per registrare la data di registrazione dello studente
2. Consentire all'utente di selezionare la data di registrazione con il widget di JQuery UI Datepicker
3. Applicare le regole di convalida per la data di registrazione

Il widget di JQuery UI Datepicker consente agli utenti di selezionare con facilità una data da un calendario che viene visualizzato quando l'utente interagisce con il campo. Uso di questo widget può essere più pratica per gli utenti che manualmente digitando una data. L'integrazione di widget Datepicker in una pagina che utilizza l'associazione di modelli per le operazioni di dati richiede solo una piccola quantità di operazioni aggiuntive.

## <a name="add-a-new-property-to-the-model"></a>Aggiungere una nuova proprietà al modello

In primo luogo, si aggiungerà un **Datetime** tuoi studenti la proprietà del modello ed eseguire la migrazione di tale modifica al database. Aprire **UniversityModels.cs**e aggiungere il codice evidenziato per il modello Student.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Il **RangeAttribute** è incluso per applicare le regole di convalida per la proprietà. Per questa esercitazione si presuppone che Contoso University fu fondata sul 1 ° gennaio 2013 e quindi le date di iscrizione precedenti non sono valide.

Nella finestra Gestione pacchetti, aggiungere una migrazione eseguendo il comando **AddEnrollmentDate migrazione aggiungere**. Si noti che il codice di migrazione aggiunge la nuova colonna di data/ora per la tabella Student. Per corrispondere al valore specificato nella RangeAttribute, aggiungere un valore predefinito per la nuova colonna, come illustrato nel codice evidenziato seguente.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Salvare la modifica del file di migrazione.

Non è necessaria inizializzare nuovamente i dati. Pertanto, aprire **Configuration.cs** nella cartella Migrations e rimuovere o impostare come commento il codice nel **Seed** (metodo). Salvare e chiudere il file.

A questo punto, eseguire il comando **update-database**. Si noti che la colonna è ora presente nel database e tutti i record esistenti hanno il valore predefinito per un elemento EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Aggiungere controlli dinamici per la data di registrazione

A questo punto si aggiungerà i controlli per visualizzare e modificare la data di registrazione. A questo punto, il valore viene modificato tramite una casella di testo. Più avanti nell'esercitazione si modificherà la casella di testo per il widget di JQuery Datepicker.

In primo luogo, è importante notare che non è necessario apportare modifiche per il **AddStudent.aspx** file. Il controllo DynamicEntity forniranno automaticamente la nuova proprietà.

Aprire **Students.aspx**e aggiungere il codice evidenziato seguente.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Eseguire l'applicazione e si noti che è possibile impostare il valore della data di registrazione digitando una data. Quando si aggiunge un nuovo studente:

![impostare la data](integrating-jquery-ui/_static/image1.png)

In alternativa, la modifica di un valore esistente:

![Data modifica](integrating-jquery-ui/_static/image2.png)

Digitare il funzionamento della data, ma potrebbe non essere l'esperienza dei clienti che si desidera fornire. Nella sezione successiva, si abiliterà la selezione di una data tramite un calendario.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installare il pacchetto NuGet per lavorare con JQuery UI

Il **succo di UI** pacchetto NuGet consente una facile integrazione dei widget JQuery UI nell'applicazione web. Per usare questo pacchetto, l'installazione tramite NuGet.

![aggiungere l'interfaccia utente di succo di](integrating-jquery-ui/_static/image3.png)

La versione dell'interfaccia utente di succo di cui si installa sia in conflitto con la versione di JQuery nell'applicazione. Prima di procedere con questa esercitazione, provare a eseguire l'applicazione. Se si verifica un errore di JavaScript, è necessario risolvere le differenze tra la versione di JQuery. È possibile aggiungere la versione prevista di JQuery nella cartella degli script (versione 1.8.2 al momento della stesura di questa esercitazione), oppure specificare il percorso al file JQuery in Site. master.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizzare il modello di data/ora in modo da includere Datepicker widget

Si aggiungerà il widget Datepicker al modello di dati dinamici per la modifica di un valore datetime. Quando si aggiungono widget per il modello, viene visualizzato automaticamente nel modulo per l'aggiunta di un nuovo studente e nella visualizzazione griglia per gli studenti di modifica. Aprire **data/ora\_Edit. ascx**e aggiungere il codice evidenziato seguente.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Nel file code-behind, si imposterà le date minime e massime per il controllo DatePicker. Impostando questi valori, si impedirà agli utenti di passare a date non valide. Si recupererà i valori minimi e massimo dal **RangeAttribute** sulla proprietà DateTime, se presente. Open **data/ora\_Edit.ascx.cs**, quindi aggiungere il codice evidenziato seguente alla pagina\_metodo Load.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Eseguire l'applicazione web e passare alla pagina AddStudent. Specificare i valori per i campi e notare che, quando si fa clic sulla casella di testo per la data di registrazione, viene visualizzato il calendario.

![selezione data](integrating-jquery-ui/_static/image4.png)

Selezionare una data e fare clic su **Inserisci**. Il RangeAttribute applica la convalida nel server. Impostando la proprietà minDate sul controllo Datepicker, è inoltre applicare la convalida sul client. Il calendario non consentire all'utente di passare a una data prima il valore della proprietà minDate.

Quando si modifica un record nella visualizzazione griglia, viene visualizzato anche il calendario.

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato illustrato come incorporare un widget di JQuery in un web form che usa l'associazione di modelli.

Entro i prossimi [esercitazione](using-query-string-values-to-retrieve-data.md), si userà un valore di stringa di query quando si selezionano i dati.

> [!div class="step-by-step"]
> [Precedente](sorting-paging-and-filtering-data.md)
> [Successivo](using-query-string-values-to-retrieve-data.md)
