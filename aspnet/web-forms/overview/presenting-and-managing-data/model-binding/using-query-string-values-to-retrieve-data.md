---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e web form | Documenti Microsoft
author: tfitzmac
description: "Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.
> 
> In questa esercitazione viene illustrato come passare un valore nella stringa di query e utilizzare tale valore per recuperare i dati tramite l'associazione di modelli.
> 
> Questa esercitazione si basa sul progetto creato nel [precedenti](retrieving-data.md) parti della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Occorre invece compilare

In questa esercitazione, effettuare le operazioni seguenti:

1. Aggiungere una nuova pagina per mostrare i corsi registrati per uno studente
2. Recuperare i corsi registrati per gli studenti selezionato in base a un valore nella stringa di query
3. Aggiungere un collegamento ipertestuale con un valore di stringa di query dalla visualizzazione griglia alla nuova pagina

I passaggi in questa esercitazione sono abbastanza simili a quello eseguito in precedenza [esercitazione](sorting-paging-and-filtering-data.md) per filtrare gli studenti visualizzati in base alla selezione utente nell'elenco a discesa. In tale esercitazione, è stato utilizzato il **controllo** attributo del metodo select per specificare che il valore del parametro provenga da un controllo. In questa esercitazione si userà il **QueryString** attributo del metodo select per specificare che il valore del parametro proviene dalla stringa di query.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Aggiungere una nuova pagina per visualizzare un corsi

Aggiungere un nuovo web form che utilizza la pagina master Site. master e denominare la pagina **corsi**.

Nel **Courses.aspx** file, aggiungere una visualizzazione griglia per visualizzare i corsi per studente selezionato.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definire il metodo select

In **Courses.aspx.cs**, si aggiungerà il metodo select con il nome specificato nella visualizzazione griglia **SelectMethod** proprietà. In quel metodo, verrà definito la query per il recupero di un corsi e specificare che il parametro proviene da un valore di stringa di query con lo stesso nome come parametro.

In primo luogo, è necessario aggiungere la seguente **utilizzando** istruzioni.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Quindi, aggiungere il codice seguente Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

L'attributo di stringa di query indica che un valore di stringa di query denominato StudentID viene automaticamente assegnato al parametro di questo metodo.

## <a name="add-hyperlink-with-query-string-value"></a>Aggiungere un collegamento ipertestuale con il valore di stringa di query

Nella visualizzazione griglia in Students.aspx, si aggiungerà un campo collegamento ipertestuale che si collega alla nuova pagina corsi. Il collegamento ipertestuale includerà un valore di stringa di query con l'id dello studente.

In Students.aspx, aggiungere il seguente campo per le colonne della vista griglia sotto il campo per crediti totale.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Eseguire l'applicazione e si noti che la visualizzazione griglia include ora il collegamento di corsi.

![Aggiungere un collegamento ipertestuale](using-query-string-values-to-retrieve-data/_static/image1.png)

Quando si fa clic su uno dei collegamenti, si noterà che registrati corsi.

![mostrare i corsi](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione, è stato aggiunto un collegamento con un valore di stringa di query. Il valore di stringa di query è utilizzato per il valore del parametro del metodo select.

Nella prossima [esercitazione](adding-business-logic-layer.md), si sposta il codice dal file code-behind in un livello di logica di business e un livello di accesso ai dati.

>[!div class="step-by-step"]
[Precedente](integrating-jquery-ui.md)
[Successivo](adding-business-logic-layer.md)
