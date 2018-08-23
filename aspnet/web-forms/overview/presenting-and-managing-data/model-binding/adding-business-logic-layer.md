---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Livello di logica di business aggiunta a un progetto che usa l'associazione di modelli e web form | Microsoft Docs
author: tfitzmac
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 2c54c0a635c662c8740aa7f9b6bd02cd71913e24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838708"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Livello di logica di business aggiunta a un progetto che usa l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come usare l'associazione di modelli con un livello di logica di business. Si imposterà il membro OnCallingDataMethods per specificare che un oggetto diverso dalla pagina corrente viene usato per chiamare i metodi di dati.
> 
> Questa esercitazione si basa sul progetto creato nel [precedenti](retrieving-data.md) parti della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

Associazione di modelli consente di inserire il codice dell'interazione tra i dati nel file code-behind per una pagina web o in una classe per la logica di business separato. Nelle esercitazioni precedenti hanno illustrato come utilizzare i file code-behind per il codice di interazione dei dati. Questo approccio funziona per i siti di piccole dimensioni, ma può generare codice di ripetizione e maggiore difficoltà durante la gestione di un sito di grandi dimensioni. Può anche essere molto difficile testare il codice che si trova nel code-behind file poiché non esiste alcun livello di astrazione a livello di codice.

Per centralizzare il codice di interazione dei dati, è possibile creare un livello di logica di business che contiene tutta la logica per l'interazione con i dati. È quindi possibile chiamare il livello di logica di business dalle pagine web. Questa esercitazione illustra come spostare tutto il codice che state scritte nelle esercitazioni precedenti in un livello di logica di business e quindi usare tale codice dalle pagine.

In questa esercitazione, sarà:

1. Spostare il codice da file code-behind a un livello di logica di business
2. Modificare i controlli associati a dati per chiamare i metodi nel livello di logica di business

## <a name="create-business-logic-layer"></a>Creare il livello di logica di business

A questo punto, si creerà la classe che viene chiamata dalle pagine web. I metodi in questa classe è un aspetto simili ai metodi che nelle esercitazioni precedenti è stato utilizzato e includono gli attributi di provider di valore.

In primo luogo, aggiungere una nuova cartella denominata **BLL**.

![Aggiungi cartella](adding-business-logic-layer/_static/image1.png)

Nella cartella di livello BLL, creare una nuova classe denominata **SchoolBL.cs**. Conterrà tutte le operazioni di dati che si trovavano originariamente nel file code-behind. I metodi sono quasi identici, ai metodi nel file code-behind, ma includeranno alcune modifiche.

La modifica più importante da notare è che non si eseguono il codice dall'interno di un'istanza di **pagina** classe. La classe di pagina contiene le **TryUpdateModel** metodo e il **ModelState** proprietà. Quando questo codice viene spostato in un livello di logica di business, non è più necessario un'istanza della classe della pagina per chiamare questi membri. Per risolvere questo problema, è necessario aggiungere un **ModelMethodContext** parametro a qualsiasi metodo che accede a ModelState o TryUpdateModel. Usare questo parametro ModelMethodContext chiamare TryUpdateModel o recuperare ModelState. Non devi modificare nulla nella pagina web per conto di questo nuovo parametro.

Sostituire il codice in SchoolBL.cs con il codice seguente.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Rivedere le pagine esistenti per recuperare i dati dal livello di logica di business

Infine, si convertirà le pagine Students.aspx AddStudent.aspx e Courses.aspx rispetto all'uso di query nel file code-behind a utilizzando il livello di logica di business.

Nel file code-behind per studenti, AddStudent e corsi, eliminare o impostare come commento i metodi di query seguenti:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

È ora non necessario alcun codice nel file code-behind che si riferisce alle operazioni sui dati.

Il **OnCallingDataMethods** gestore eventi consente di specificare un oggetto da utilizzare per i metodi di dati. In Students.aspx, aggiungere un valore per tale gestore dell'evento e modificare i nomi dei metodi dei dati per i nomi dei metodi nella classe per la logica di business.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Nel file code-behind per Students.aspx, definire il gestore eventi per l'evento CallingDataMethods. In questo gestore eventi, specificare la classe per la logica di business per le operazioni sui dati.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

In AddStudent.aspx, apportare modifiche analoghe.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

In Courses.aspx, apportare modifiche analoghe.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Eseguire l'applicazione e si noti che tutte le pagine di funzioni che avevano in precedenza. La logica di convalida funziona inoltre correttamente.

## <a name="conclusion"></a>Conclusione

In questa esercitazione, è strutturata nuovamente l'applicazione per usare un livello di accesso ai dati e il livello di logica di business. È stato specificato che i controlli dei dati usano un oggetto che non è la pagina corrente per le operazioni sui dati.

> [!div class="step-by-step"]
> [Precedente](using-query-string-values-to-retrieve-data.md)
