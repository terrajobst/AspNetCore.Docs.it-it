---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Utilizza CascadingDropDown con un Database (c#) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 1991c26d408e593999288ea6df0467cea0369457
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-cascadingdropdown-with-a-database-c"></a>Utilizza CascadingDropDown con un Database (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. Affinché il funzionamento, è necessario creare un servizio web speciale.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Affinché il funzionamento, è necessario creare un servizio web speciale.

## <a name="steps"></a>Passaggi

Prima di tutto, un'origine dati è obbligatoria. Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il &lt; `form` &gt; elemento):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

Nel passaggio successivo, sono necessari due controlli DropDownList. In questo esempio, viene usato il fornitore e informazioni di contatto da AdventureWorks, pertanto è creare un unico elenco per i fornitori disponibili e uno per i contatti disponibili:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

Quindi, è necessario aggiungere due Extender CascadingDropDown alla pagina. Uno compilato automaticamente il primo elenco (fornitori), e l'altro nel secondo elenco (contatti). Impostare gli attributi seguenti:

- `ServicePath`: URL di un servizio web recapitare le voci dell'elenco
- `ServiceMethod`: Metodo web recapito le voci dell'elenco
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: Informazioni sulle categorie viene inviati al metodo web quando viene chiamato
- `PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati elenco dal server
- `ParentControlID`: (facoltativo) che il caricamento di trigger dell'elenco corrente di elenco a discesa di padre

A seconda del linguaggio di programmazione utilizzato, viene modificato il nome del servizio web in questione, ma tutti gli altri valori di attributo sono uguali. Di seguito è l'elemento CascadingDropDown per il primo elenco a discesa:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

È necessario impostare le estensioni di controllo per l'elenco secondo il `ParentControlID` attributo in modo che la selezione di una voce nei trigger elenco fornitori durante il caricamento associati a elementi dell'elenco di contatti.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

Il lavoro effettivo viene quindi eseguito nel servizio web, che è impostato come indicato di seguito. Si noti che il `[ScriptService]` attributo viene utilizzato, in caso contrario AJAX ASP.NET non è possibile creare il proxy JavaScript per accedere ai metodi web dal codice di script sul lato client.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

La firma dei metodi web chiamato da CascadingDropDown è come segue:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Pertanto il valore restituito deve essere una matrice di tipo `CascadingDropDownNameValue` definito da Control Toolkit. Il `GetVendors()` metodo è piuttosto semplice da implementare: il codice si connette al database AdventureWorks e i primi 25 fornitori una query. Il primo parametro di `CascadingDropDownNameValue` costruttore è la didascalia della voce di elenco, il secondo valore (attributo value in del formato HTML &lt; `option` &gt; elemento). Ecco il codice:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Ottenere i contatti associati per un fornitore (nome del metodo: `GetContactsForVendor()`) è leggermente più complessi. Prima di tutto, è necessario determinare il fornitore a cui è stato selezionato nel primo elenco a discesa. Control Toolkit definisce un metodo di supporto per l'attività: la `ParseKnownCategoryValuesString()` metodo restituisce un `StringDictionary` elemento con i dati dell'elenco a discesa:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Per motivi di sicurezza, questi dati devono essere convalidati prima. Pertanto, se è presente una voce del fornitore (perché il `Category` del primo elemento CascadingDropDown è impostata su `"Vendor"`), è possibile recuperare l'ID del fornitore selezionato:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

Il resto del metodo è piuttosto semplice, quindi. ID del fornitore viene utilizzato come parametro per una query SQL che recupera tutti i contatti associati di tale fornitore. In questo caso, il metodo restituisce una matrice di tipo `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

Caricare la pagina ASP.NET e, dopo un breve periodo di tempo nell'elenco di fornitori viene compilato con le voci di 25. Selezionare una voce e notare come secondo elenco a discesa viene riempita con i dati.


[![Nel primo elenco viene compilato automaticamente](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

Nel primo elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine ingrandita](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![Nel secondo elenco viene compilato in base della selezione nel primo elenco](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

Nel secondo elenco viene compilato in base alla selezione nel primo elenco ([fare clic per visualizzare l'immagine ingrandita](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](filling-a-list-using-cascadingdropdown-cs.md)
> [Successivo](presetting-list-entries-with-cascadingdropdown-cs.md)
