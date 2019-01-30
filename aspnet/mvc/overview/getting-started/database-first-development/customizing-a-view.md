---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Esercitazione: Personalizzare la visualizzazione per Entity Framework Database First con app ASP.NET MVC'
description: Questa esercitazione è incentrata su come modificare le visualizzazioni generate automaticamente per migliorare la presentazione.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236497"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: Personalizzare la visualizzazione per Entity Framework Database First con app ASP.NET MVC

Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.

Questa esercitazione è incentrata su come modificare le visualizzazioni generate automaticamente per migliorare la presentazione.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere corsi alla pagina di dettaglio per studenti
> * Verificare che i corsi vengono aggiunti alla pagina

## <a name="prerequisites"></a>Prerequisiti

* [Modificare il database](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Aggiungere corsi alla dettagli dello studente

Il codice generato fornisce un buon punto di partenza per l'applicazione, ma non necessariamente fornisce tutte le funzionalità necessarie nell'applicazione. È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione. Attualmente, l'applicazione non visualizza i corsi registrati dello studente selezionato. In questa sezione si aggiungerà i corsi registrati per ogni studente per il **dettagli** vista per gli studenti.

Aprire **viste** > **studenti** > *Details. cshtml*. Sotto l'ultima &lt;/dl&gt; tag, ma prima della chiusura &lt;/div&gt; tag, aggiungere il codice seguente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Questo codice crea una tabella che visualizza una riga per ogni record della tabella Enrollment dello studente selezionato. Il **Display** metodo esegue il rendering HTML per l'oggetto (modelItem) che rappresenta l'espressione. Si usa il metodo di visualizzazione (anziché semplicemente incorporare il valore della proprietà nel codice) per assicurarsi che il valore viene formattato correttamente in base del tipo e il modello per tale tipo. In questo esempio, ogni espressione restituisce una singola proprietà del record corrente nel ciclo e i valori sono i tipi primitivi che vengono visualizzati come testo.

## <a name="confirm-courses-are-added"></a>Confermare i corsi vengono aggiunti

Eseguire la soluzione. Fare clic su **elenco degli studenti** e selezionare **dettagli** per uno degli studenti. Si noterà che i corsi registrati sono stati inclusi nella vista.

![studente con la registrazione](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Passaggi successivi
Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Corsi aggiunti alla pagina di dettaglio per studenti
> * Conferma che i corsi vengono aggiunti alla pagina

Passare all'esercitazione successiva per informazioni su come aggiungere le annotazioni dei dati per specificare i requisiti della convalida e formattazione di visualizzazione.
> [!div class="nextstepaction"]
> [Migliorare la convalida dei dati](enhancing-data-validation.md)
