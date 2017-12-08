---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Database di Entity Framework prima con ASP.NET MVC: personalizzare una visualizzazione | Documenti Microsoft'
author: tfitzmac
description: "Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>Database di Entity Framework prima con ASP.NET MVC: personalizzare una visualizzazione
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrata sulla modifica delle viste generati automaticamente per migliorare la presentazione.


## <a name="add-enrolled-courses-to-student-details"></a>Aggiungi corsi registrati dettagli per studenti

Il codice generato fornisce un buon punto di partenza per l'applicazione ma non necessariamente fornisce tutte le funzionalità necessarie nell'applicazione. È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione. Attualmente, l'applicazione non vengono visualizzati i corsi registrati per gli studenti selezionato. In questa sezione si aggiungeranno i corsi registrati per ogni studente per il **dettagli** visualizzazione per gli studenti.

Aprire **Students/Details.cshtml**e sotto l'ultima &lt;/dl&gt; scheda, ma prima della chiusura &lt;/div&gt; tag, aggiungere il codice seguente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Questo codice crea una tabella che visualizza una riga per ogni record nella tabella di registrazione per gli studenti selezionato. Il **visualizzazione** metodo esegue il rendering HTML per l'oggetto (modelItem) che rappresenta l'espressione. Utilizzare il metodo di visualizzazione (invece di incorporamento semplicemente il valore della proprietà nel codice) per assicurarsi che il valore viene formattato correttamente in base a tipo e il modello per quel tipo. In questo esempio, ogni espressione restituisce una singola proprietà del record corrente del ciclo e i valori sono i tipi primitivi che vengono visualizzati come testo.

Ripetere la ricerca per la visualizzazione di studenti/indice e selezionare **dettagli** per uno degli studenti. Verranno visualizzati che i corsi registrati sono stati inclusi nella vista.

![studente con la registrazione](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
[Precedente](changing-the-database.md)
[Successivo](enhancing-data-validation.md)
