---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: L'integrazione con l'associazione di modelli e web form JQuery UI Datepicker | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>L'integrazione di JQuery UI Datepicker con l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.
> 
> In questa esercitazione viene illustrato come aggiungere la UI JQuery [Datepicker widget](http://jqueryui.com/datepicker/) in un Web Form e l'utilizzo del modello di associazione per aggiornare il database con il valore selezionato.
> 
> Questa esercitazione si basa sul progetto creato nel [prima](retrieving-data.md) e [secondo](updating-deleting-and-creating-data.md) parti della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Occorre invece compilare

In questa esercitazione, effettuare le operazioni seguenti:

1. Aggiungere una proprietà al modello per registrare la data di registrazione dello studente
2. Consentire all'utente di selezionare la data di registrazione utilizzando il widget JQuery UI Datepicker
3. Applicare le regole di convalida per la data di registrazione

Il widget JQuery UI Datepicker consente agli utenti di selezionare con facilità una data dal calendario che viene visualizzato quando l'utente interagisce con il campo. Utilizzo di questo widget può essere più conveniente per gli utenti di manualmente digitando una data. Integrazione di widget Datepicker in una pagina che utilizza l'associazione di modelli per le operazioni di dati richiede solo una piccola quantità di lavoro aggiuntiva.

## <a name="add-a-new-property-to-the-model"></a>Aggiungere una nuova proprietà al modello

In primo luogo, si aggiungerà un **Datetime** proprietà per il vostro studente del modello ed eseguire la migrazione di tale modifica al database. Aprire **UniversityModels.cs**e aggiungere il codice evidenziato per il modello di studenti.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Il **RangeAttribute** è incluso per applicare le regole di convalida per la proprietà. Per questa esercitazione, si suppone che Contoso University fondata sul 1 ° gennaio 2013 e pertanto non sono valide le date di registrazione precedente.

Nella finestra di gestione dei pacchetti, aggiungere una migrazione eseguendo il comando **AddEnrollmentDate migrazione aggiungere**. Si noti che il codice di migrazione aggiunge la nuova colonna Datetime nella tabella di studenti. Per trovare il valore specificato è il RangeAttribute, aggiungere un valore predefinito per la nuova colonna, come illustrato nel seguente codice evidenziato.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Salvare la modifica del file di migrazione.

Non è necessario inizializzare nuovamente i dati. Pertanto, aprire **Configuration.cs** nella cartella migrazioni e rimuovere o impostare come commento il codice il **valore di inizializzazione** (metodo). Salvare e chiudere il file.

A questo punto, eseguire il comando **aggiornamento database**. Si noti che la colonna è ora presente nel database e di tutti i record esistenti il valore predefinito per EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Aggiungere i controlli dinamici per la data di registrazione

A questo punto si aggiungerà i controlli per visualizzare e modificare la data di registrazione. A questo punto, il valore viene modificato tramite una casella di testo. Più avanti nell'esercitazione si modificherà la casella di testo per il widget JQuery Datepicker.

Innanzitutto, è importante notare che è necessario apportare qualsiasi modifica apportata al **AddStudent.aspx** file. Il controllo DynamicEntity renderà automaticamente la nuova proprietà.

Aprire **Students.aspx**e aggiungere il codice evidenziato di seguito.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Eseguire l'applicazione e si noti che è possibile impostare il valore della data di registrazione digitando una data. Quando si aggiunge un nuovo studente:

![impostare la data](integrating-jquery-ui/_static/image1.png)

In alternativa, la modifica di un valore esistente:

![Data modifica](integrating-jquery-ui/_static/image2.png)

Digitare la data di funzioni, ma potrebbe non essere si desidera fornire l'esperienza del cliente. Nella sezione successiva, verrà abilitata la selezione di una data tramite un calendario.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installare il pacchetto NuGet per funzionare con JQuery UI

Il **dell'interfaccia utente succo** pacchetto NuGet consente una facile integrazione dei widget JQuery UI nell'applicazione web. Per utilizzare questo pacchetto, installarlo tramite NuGet.

![aggiunta dell'interfaccia utente di succo di](integrating-jquery-ui/_static/image3.png)

La versione dell'interfaccia utente di succo installato sia in conflitto con la versione di JQuery nell'applicazione. Prima di procedere con questa esercitazione, provare a eseguire l'applicazione. Se si verifica un errore di JavaScript, è necessario risolvere le differenze tra la versione di JQuery. È possibile aggiungere la versione prevista di JQuery alla cartella degli script (versione 1.8.2 al momento della stesura di questa esercitazione) o in Site. master, specificare il percorso al file JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizzare il modello di data/ora per includere i widget Datepicker

Il widget Datepicker verranno aggiunti al modello di dati dinamici per la modifica di un valore datetime. Aggiungendo il widget per il modello, ne viene automaticamente eseguito il rendering nel formato per l'aggiunta di un nuovo studente e nella visualizzazione griglia per studenti di modifica. Aprire **DateTime\_Edit. ascx**e aggiungere il codice evidenziato di seguito.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Nel file code-behind, si imposteranno le date minime e massime per la selezione data. Impostando questi valori, si impedirà agli utenti di spostarsi tra le date non valide. Si recupererà i valori minimo e massimi dal **RangeAttribute** sulla proprietà DateTime, se disponibile. Aprire **DateTime\_Edit.ascx.cs**e aggiungere il codice evidenziato di seguito alla pagina\_metodo Load.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Eseguire l'applicazione web e passare alla pagina AddStudent. Specificare i valori per i campi e notare che, quando si fa clic sulla casella di testo per la data di registrazione, viene visualizzato il calendario.

![selezione data](integrating-jquery-ui/_static/image4.png)

Selezionare una data e fare clic su **inserire**. Il RangeAttribute applica la convalida sul server. Impostando la proprietà minDate sul Datepicker, è inoltre applicare la convalida sul client. Il calendario non consentire all'utente di passare a una data precedente il valore della proprietà minDate.

Quando si modifica un record nella visualizzazione griglia, viene visualizzato anche il calendario.

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato descritto come incorporare un widget di JQuery in un web form che utilizza l'associazione del modello.

Nella prossima [esercitazione](using-query-string-values-to-retrieve-data.md), si utilizzerà un valore di stringa di query quando la selezione di dati.

> [!div class="step-by-step"]
> [Precedente](sorting-paging-and-filtering-data.md)
> [Successivo](using-query-string-values-to-retrieve-data.md)
