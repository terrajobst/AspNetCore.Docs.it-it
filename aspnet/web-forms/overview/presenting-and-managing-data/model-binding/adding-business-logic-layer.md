---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Livello di logica di business aggiunta a un progetto che utilizza l'associazione di modelli e web form | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892751"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Livello di logica di business aggiunta a un progetto che utilizza l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.
> 
> In questa esercitazione viene illustrato come utilizzare l'associazione di modelli con un livello di logica di business. Impostare il membro OnCallingDataMethods per specificare che un oggetto diverso dalla pagina corrente viene utilizzato per chiamare i metodi di dati.
> 
> Questa esercitazione si basa sul progetto creato nel [precedenti](retrieving-data.md) parti della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Occorre invece compilare

Associazione di modelli consente di inserire il codice di interazione dei dati in entrambi i file code-behind per una pagina web o in una classe della logica di business separato. Nelle esercitazioni precedenti hanno illustrato come utilizzare i file code-behind per il codice di interazione dei dati. Questo approccio funziona per i siti di piccole dimensioni, ma può causare il codice di ripetizione e maggiore difficoltà per la gestione dei siti di grandi dimensioni. Può anche essere molto difficile da test a livello di programmazione del codice che si trova nel file di codice sottostante perché non esiste alcun livello di astrazione.

Per centralizzare il codice di interazione dei dati, è possibile creare un livello di logica di business che contiene la logica per l'interazione con i dati. Chiamare quindi il livello di logica di business dalle pagine web. In questa esercitazione viene illustrato come spostare tutto il codice che scritto nelle esercitazioni precedenti in un livello di logica di business e quindi utilizzare tale codice dalle pagine.

In questa esercitazione, effettuare le operazioni seguenti:

1. Spostare il codice dal file code-behind per un livello di logica di business
2. Modificare i controlli con associazione a dati per chiamare i metodi nel livello di logica di business

## <a name="create-business-logic-layer"></a>Creare il livello di logica di business

A questo punto, si creerà la classe che viene chiamata dalle pagine web. I metodi in questa classe simile ai metodi utilizzati nelle esercitazioni precedenti e includono gli attributi di provider di valore.

Aggiungere innanzitutto una nuova cartella denominata **BLL**.

![aggiungere la cartella](adding-business-logic-layer/_static/image1.png)

Nella cartella BLL, creare una nuova classe denominata **SchoolBL.cs**. Contiene tutte le operazioni di dati che si trovavano originariamente nel file code-behind. I metodi sono quasi identici, ai metodi nel file code-behind, ma includeranno alcune modifiche.

La modifica più importante da notare è che sono non più in esecuzione il codice all'interno di un'istanza di **pagina** classe. Contiene la classe delle pagine di **TryUpdateModel** (metodo) e **ModelState** proprietà. Quando questo codice viene spostato in un livello di logica di business, non è un'istanza della classe di pagina per chiamare i membri. Per evitare questo problema, è necessario aggiungere un **ModelMethodContext** parametro a un metodo che accede a TryUpdateModel o ModelState. Utilizzare questo parametro ModelMethodContext per chiamare TryUpdateModel o recuperare ModelState. Non è necessario modificare qualsiasi elemento nella pagina web per conto di questo nuovo parametro.

Sostituire il codice in SchoolBL.cs con il codice seguente.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Rivedere le pagine esistenti per recuperare i dati dal livello di logica di business

Infine, si convertirà pagine Students.aspx AddStudent.aspx e Courses.aspx dall'utilizzo di query nel file code-behind per utilizzando il livello di logica di business.

Nel file code-behind per studenti e AddStudent corsi, eliminare o impostare come commento i metodi di query seguenti:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

È ora non necessario alcun codice nel file di codice relativo alle operazioni sui dati.

Il **OnCallingDataMethods** gestore eventi consente di specificare un oggetto da utilizzare per i metodi di dati. In Students.aspx, aggiungere un valore per tale gestore eventi e modificare i nomi dei metodi dei dati per i nomi dei metodi nella classe della logica di business.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Nel file code-behind per Students.aspx, definire il gestore eventi per l'evento CallingDataMethods. In questo gestore eventi, specificare la classe di logica di business per le operazioni sui dati.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

In AddStudent.aspx, apportare modifiche analoghe.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

In Courses.aspx, apportare modifiche analoghe.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Eseguire l'applicazione e si noti che tutte le pagine di funzioni che avevano in precedenza. La logica di convalida funziona inoltre correttamente.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è strutturato nuovamente l'applicazione di utilizzare un livello di accesso ai dati e il livello di logica di business. È stato specificato che i controlli di dati utilizzano un oggetto che non è la pagina corrente per le operazioni sui dati.

> [!div class="step-by-step"]
> [Precedente](using-query-string-values-to-retrieve-data.md)
