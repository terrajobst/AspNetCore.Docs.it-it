---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Database di Entity Framework prima di tutto con ASP.NET MVC: personalizzare una vista | Microsoft Docs'
author: Rick-Anderson
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: f66e097d53514ab3842e04cd545ca626c652478a
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021210"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>Database di Entity Framework prima di tutto con ASP.NET MVC: personalizzare una vista
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrato su come modificare le visualizzazioni generate automaticamente per migliorare la presentazione.


## <a name="add-enrolled-courses-to-student-details"></a>Aggiungere corsi registrati i dettagli per studenti

Il codice generato fornisce un buon punto di partenza per l'applicazione, ma non necessariamente fornisce tutte le funzionalità necessarie nell'applicazione. È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione. Attualmente, l'applicazione non visualizza i corsi registrati dello studente selezionato. In questa sezione si aggiungerà i corsi registrati per ogni studente per il **dettagli** vista per gli studenti.

Aprire **Students/Details.cshtml**e sotto l'ultimo &lt;/dl&gt; scheda, ma prima della chiusura &lt;/div&gt; tag, aggiungere il codice seguente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Questo codice crea una tabella che visualizza una riga per ogni record della tabella Enrollment dello studente selezionato. Il **Display** metodo esegue il rendering HTML per l'oggetto (modelItem) che rappresenta l'espressione. Si usa il metodo di visualizzazione (anziché semplicemente incorporare il valore della proprietà nel codice) per assicurarsi che il valore viene formattato correttamente in base del tipo e il modello per tale tipo. In questo esempio, ogni espressione restituisce una singola proprietà del record corrente nel ciclo e i valori sono i tipi primitivi che vengono visualizzati come testo.

Ripetere la ricerca alla visualizzazione Students/Index e selezionare **dettagli** per uno degli studenti. Si noterà che i corsi registrati sono stati inclusi nella vista.

![studente con la registrazione](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Precedente](changing-the-database.md)
> [Successivo](enhancing-data-validation.md)
