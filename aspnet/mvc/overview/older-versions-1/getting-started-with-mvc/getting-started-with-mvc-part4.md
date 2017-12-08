---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Creazione di un Database | Documenti Microsoft
author: shanselman
description: "Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 6a9617b8056f883d6be2547588b13063a58abd0e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-database"></a>Creazione di un Database
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione verrà per creare un nuovo database di SQL Express che verranno utilizzate per archiviare e recuperare i dati di film. IDE di Visual Web Developer, selezionare visualizzazione | Esplora server. Fare clic con il pulsante destro su connessioni di dati e fare clic su Aggiungi connessione...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Nella finestra di dialogo Scegli origine dati, selezionare Microsoft SQL Server e selezionare continua.

![](getting-started-with-mvc-part4/_static/image2.png)

Nella finestra di dialogo Aggiungi connessione immettere ". \SQLEXPRESS" per il nome del Server, quindi immettere "Filmati" come nome per il nuovo database.

[![Finestra di dialogo di connessione aggiunta](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Fare clic su OK e viene chiesto se si desidera creare il database. Selezionare Sì.

[![Creare filmati?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Ora si ha un database vuoto in Esplora Server.

![Aggiungi nuova tabella](getting-started-with-mvc-part4/_static/image7.png)

Fare clic con il pulsante destro su tabelle e fare clic su Aggiungi tabella. Verrà visualizzata la finestra di progettazione di tabella. Aggiungere colonne per Id, titolo, ReleaseDate, Genre e prezzo. Fare clic con il pulsante destro sulla colonna ID e fare clic su Imposta chiave primaria. Ecco le my aree di progettazione ha un aspetto simile.

[![Editor di tabella di database](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Inoltre, selezionare la colonna Id e in proprietà della colonna seguito modificare "Specifica identità" a "Yes".

[![IsIdentity - proprietà di colonna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Quando hai eseguito, fare clic sull'icona Salva nella barra degli strumenti o selezionare File | Nome della tabella e Salva dal menu "**film**" (singolare). Abbiamo un database e una tabella.

[![Scegliere nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Tornare a Esplora Server e fare clic con il pulsante destro nella tabella di film, quindi selezionare "Mostra tabella dati". Immettere alcuni filmati database dispone di dati.

[![Modifica delle tabelle di database](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Creazione di un modello

A questo punto, tornare a Esplora soluzioni sul lato destro dell'IDE e destro del mouse sulla cartella modelli e selezionare Aggiungi | Nuovo elemento.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Si userà per creare un modello di entità dal nuovo database. Verrà aggiunto un set di classi al progetto che rende più semplice per la query e modificare i dati all'interno del database. Selezionare il nodo di dati sul lato sinistro della finestra di dialogo e quindi selezionare il modello di elemento di ADO.NET Entity Data Model. Nome Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Fare clic sul pulsante "Aggiungi". Viene quindi avviata "Entity Data Model Wizard".

Nella finestra di dialogo nuovo visualizzata selezionare Genera da Database. Poiché sono stati apportati solo un database, è necessario solo informare che Entity Framework il nuovo database e la relativa tabella. Scegliere Avanti per salvare la connessione al database nella configurazione dell'applicazione web. A questo punto, controllare le tabelle e un film casella di controllo e fare clic su Fine.

[![Procedura guidata Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Ora è possibile visualizzare la nuova tabella film in Entity Framework Designer e accedervi dal codice.

[![Filmati - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Nell'area di progettazione è possibile visualizzare una classe "Filmato". Questa classe esegue il mapping alla tabella nel database "Filmato" e ogni proprietà all'interno di esso viene eseguito il mapping a una colonna con la tabella. Ogni istanza di una classe "Filmato" corrisponde a una riga all'interno della tabella "Filmato".

Se non si desidera che i nomi predefiniti e mapping convenzioni usate da Entity Framework, è possibile utilizzare la finestra di progettazione di Entity Framework per modificare o personalizzare. Per questa applicazione verrà usata le impostazioni predefinite e salvare il file come-è.

È ora possibile lavorare con alcuni dati reali.

>[!div class="step-by-step"]
[Precedente](getting-started-with-mvc-part3.md)
[Successivo](getting-started-with-mvc-part5.md)
