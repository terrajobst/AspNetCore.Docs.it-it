---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creare un Database | Microsoft Docs
author: microsoft
description: Passaggio 2 mostra i passaggi per creare il database che contengono tutte le la cena e conferma la partecipazione dei dati per la nostra applicazione NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834455"
---
<a name="create-a-database"></a>Creare un Database
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 2 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 2 mostra i passaggi per creare il database che contengono tutte le la cena e conferma la partecipazione dei dati per la nostra applicazione NerdDinner.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner passaggio 2: Creazione del Database

Verrà usato un database per archiviare tutti i dati Dinner e RSVP per la nostra applicazione NerdDinner.

La procedura seguente illustra la creazione del database usando l'edizione gratuita di SQL Server Express (che è possibile installare facilmente tramite V2 del [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Tutto il codice verrà scritta funziona con SQL Server Express e la versione completa di SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Crea un nuovo database di SQL Server Express

Si sarà iniziare facendo clic con il progetto web e quindi selezionare il **Add -&gt;nuovo elemento** comando di menu:

![](create-a-database/_static/image1.png)

Verrà visualizzata una finestra di dialogo "Aggiungi nuovo elemento" Visual Studio. Verrà filtrare in base alla categoria "Dati" e selezionare il modello di elemento "Database di SQL Server":

![](create-a-database/_static/image2.png)

Denominiamo il database di SQL Server Express che vogliamo creare "NerdDinner.mdf" e fare clic su ok. Visual Studio richiederà quindi noi se si vuole aggiungere il file al nostro \App\_directory dei dati (che è già una directory di installazione con sia in lettura e scrittura ACL di sicurezza):

![](create-a-database/_static/image3.png)

Facciamo clic su "Sì" e il nuovo database verrà creato e aggiunto al nostro Esplora soluzioni:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Creazione di tabelle all'interno di Database

È ora disponibile un nuovo database vuoto. È possibile aggiungervi alcune tabelle.

A tale scopo è visualizzata finestra scheda "Esplora Server" all'interno di Visual Studio, che consente di gestire server e database. Database di SQL Server Express archiviati nel \App\_cartella dati dell'applicazione verrà visualizzato automaticamente all'interno di Esplora Server. È facoltativamente possibile usare l'icona "Connettersi al Database" nella parte superiore della finestra "Esplora Server" per aggiungere altri database di SQL Server (locali e remoti) oltre all'elenco:

![](create-a-database/_static/image5.png)

Due tabelle verrà aggiunto al database NerdDinner: uno per archiviare i nostri Dinners e l'altro per tenere traccia delle accettazioni RSVP ad essi. È possibile creare nuove tabelle facendo clic sulla cartella "Tabelle" all'interno di database e scegliendo il comando di menu "Aggiungi nuova tabella":

![](create-a-database/_static/image6.png)

Verrà aperta una finestra di progettazione di una tabella che consente di configurare lo schema della tabella. Per la tabella "Dinners" si aggiungerà 10 colonne di dati:

![](create-a-database/_static/image7.png)

Si vuole la colonna "DinnerID" sia una chiave primaria univoca per la tabella. È possibile configurare questa impostazione facendo clic sulla colonna "DinnerID" e scegliendo la voce di menu "Imposta chiave primaria":

![](create-a-database/_static/image8.png)

Oltre a rendere DinnerID una chiave primaria, è necessario anche lo configura come una colonna di "identity" il cui valore viene incrementato automaticamente man mano che nuove righe di dati vengono aggiunti alla tabella (vale a dire la prima riga Dinner inserita avrà un DinnerID pari a 1, il secondo inserito riga sarà necessario un DinnerID 2, e così via).

È possibile farlo, selezionare la colonna "DinnerID" e quindi usare l'editor "Proprietà delle colonne" per impostare la proprietà "(Is Identity)" nella colonna su "Sì". Si userà le impostazioni predefinite standard di identità (partono da 1 e incrementare di 1 in ogni nuova riga Dinner):

![](create-a-database/_static/image9.png)

La tabella verranno quindi salvate, digitando Ctrl + S oppure usando il **File -&gt;salvare** comando di menu. Verrà richiesto di assegnare un nome di tabella. Denominiamo "Dinners":

![](create-a-database/_static/image10.png)

La nuova tabella Dinners verrà quindi visualizzato entro il database in Esplora server.

È quindi possibile ripetere i passaggi precedenti e creare una tabella "RSVP". La tabella con include 3 colonne. Verranno della colonna RsvpID come chiave primaria di installazione e rendono inoltre una colonna identity:

![](create-a-database/_static/image11.png)

È possibile salvarlo e assegnare il nome "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Impostazione di una relazione di chiave esterna tra le tabelle

Ora abbiamo due tabelle all'interno di database. L'ultimo passaggio della progettazione dello schema sarà per impostare una relazione "uno-a-molti" tra queste due tabelle, in modo che è possibile associare ogni riga Dinner con zero o più righe RSVP che si applicano a esso. Faremo configurando colonna "DinnerID" della tabella RSVP per avere una relazione di chiave esterna nella colonna "DinnerID" nella tabella "Dinners".

A tale scopo che verrà aperto il contenuto della tabella RSVP all'interno di progettazione tabelle facendovi doppio clic in Esplora server. La colonna "DinnerID" all'interno di esso, quindi selezioniamo destro del mouse e scegliere il comando di menu di scelta rapida "Relationshps in corso":

![](create-a-database/_static/image12.png)

Verrà visualizzata una finestra di dialogo che è possibile usare per le relazioni di programma di installazione tra le tabelle:

![](create-a-database/_static/image13.png)

Si sarà fare clic sul pulsante "Aggiungi" per aggiungere una nuova relazione nella finestra di dialogo. Dopo aver aggiunto una relazione, si sarà espandere il nodo di visualizzazione ad albero "Specifica di tabelle e colonne" all'interno della griglia delle proprietà a destra della finestra di dialogo e quindi fare clic sul pulsante "..." a destra:

![](create-a-database/_static/image14.png)

Fare clic sul pulsante "...", verrà visualizzata un'altra finestra di dialogo che consente di specificare quali tabelle e colonne sono coinvolte nella relazione, nonché consentono di assegnare un nome della relazione.

Si verrà modificare la tabella di chiave primaria affinché sia "Dinners" e selezionare la colonna "DinnerID" all'interno della tabella Dinners come chiave primaria. La tabella RSVP sarà la tabella di chiave esterna e il RSVP. Colonna DinnerID sarà associato come la chiave esterna:

![](create-a-database/_static/image15.png)

Ora ogni riga della tabella RSVP verrà associato a una riga nella tabella cena. SQL Server verrà mantenere l'integrità referenziale per noi – e impediscono di aggiunta di una nuova riga RSVP se non fa riferimento a una riga Dinner valida. Anche impedirà noi l'eliminazione di una riga Dinner se non esistono ancora RSVP righe che fanno riferimento a esso.

### <a name="adding-data-to-our-tables"></a>Aggiunta di dati per le tabelle

È possibile completare aggiungendo alcuni dati di esempio alla nostra tabella Dinners. È possibile aggiungere dati a una tabella facendo clic su di esso in Esplora Server e scegliendo il comando "Mostra dati tabella":

![](create-a-database/_static/image16.png)

Si aggiungerà poche righe di dati Dinner che è possibile usare in un secondo momento come iniziare a implementare l'applicazione:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Passo successivo

Abbiamo completato la creazione di database. Creare ora le classi di modello che è possibile usare per eseguire una query e aggiornarlo.

> [!div class="step-by-step"]
> [Precedente](create-a-new-aspnet-mvc-project.md)
> [Successivo](build-a-model-with-business-rule-validations.md)
