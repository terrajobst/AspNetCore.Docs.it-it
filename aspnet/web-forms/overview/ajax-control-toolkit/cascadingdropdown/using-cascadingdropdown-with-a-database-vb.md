---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Uso di CascadingDropDown con un Database (VB) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichi associati i valori in anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 48e52fce4e26ec08403c3d86a4c3f85c103baa82
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824231"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Uso di CascadingDropDown con un Database (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati. Affinché il corretto funzionamento, è necessario creare un servizio web speciale.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati. (Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Affinché il corretto funzionamento, è necessario creare un servizio web speciale.

## <a name="steps"></a>Passaggi

Prima di tutto, è richiesta un'origine dati. Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la &lt; `form` &gt; elemento):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

Nel passaggio successivo, sono necessari due controlli DropDownList. In questo esempio, utilizziamo il fornitore e informazioni di contatto da AdventureWorks, pertanto creiamo un unico elenco per i fornitori disponibili e uno per i contatti disponibili:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Quindi, è necessario aggiungere due dispositivi Extender CascadingDropDown alla pagina. Uno compilato automaticamente nel primo elenco (fornitori), e un' nel secondo elenco (contatti). È necessario impostare gli attributi seguenti:

- `ServicePath`: URL di un servizio web distribuisce le voci dell'elenco
- `ServiceMethod`: Metodo web fornire le voci dell'elenco
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: Informazioni categoria in cui viene inviate al metodo web quando viene chiamato
- `PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server
- `ParentControlID`: (facoltativo) che il caricamento di trigger dell'elenco corrente nell'elenco a discesa padre

A seconda del linguaggio di programmazione utilizzato, il nome del servizio web in questione viene modificato, ma tutti gli altri valori di attributo sono lo stesso. Ecco l'elemento CascadingDropDown per il primo elenco a discesa:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

È necessario impostare i controlli Extender per l'elenco secondo il `ParentControlID` attributo in modo che se si seleziona una voce nei trigger elenco fornitori caricamento associati elementi nell'elenco di contatti.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Le operazioni effettive vengono quindi eseguite nel servizio web, che è configurato come indicato di seguito. Si noti che il `[ScriptService]` attributo viene utilizzato, in caso contrario, ASP.NET AJAX non è possibile creare il proxy di JavaScript per accedere ai metodi web dal codice di script lato client.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

La firma dei metodi web chiamato da CascadingDropDown è come segue:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

In modo che il valore restituito deve essere una matrice di tipo `CascadingDropDownNameValue` definito dal Toolkit di controllo. Il `GetVendors()` metodo è piuttosto semplice da implementare: il codice si connette al database AdventureWorks e i primi 25 fornitori una query. Il primo parametro nel `CascadingDropDownNameValue` costruttore è la didascalia della voce di elenco, il secondo il relativo valore (attributo value del linguaggio HTML &lt; `option` &gt; elemento). Ecco il codice:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Come ottenere i contatti associati per un fornitore (nome del metodo: `GetContactsForVendor()`) è un po' più complicato. Prima di tutto, è necessario determinare il fornitore che è stato selezionato nel primo elenco a discesa. Il Toolkit di controllo definisce un metodo di supporto per tale attività: il `ParseKnownCategoryValuesString()` metodo restituisce un `StringDictionary` elemento con i dati di elenco a discesa:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Per motivi di sicurezza, questi dati devono essere convalidati prima di tutto. Pertanto, se è presente una voce del fornitore (perché il `Category` del primo elemento CascadingDropDown è impostata su `"Vendor"`), può essere recuperato l'ID del fornitore selezionato:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Il resto del metodo è piuttosto semplice, quindi. ID del fornitore viene utilizzato come parametro per una query SQL che recupera tutti i contatti associati per fornitore. Ancora una volta, il metodo restituisce una matrice di tipo `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Caricare la pagina ASP.NET e dopo un breve periodo di tempo viene inserito nell'elenco di fornitori con le voci di 25. Selezionare una voce e notare come il secondo elenco a discesa viene riempita con i dati.


[![Nel primo elenco viene compilato automaticamente](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Nel primo elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni normali](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![Nel secondo elenco viene compilato in base della selezione nel primo elenco](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Nel secondo elenco viene compilato in base della selezione nel primo elenco ([fare clic per visualizzare l'immagine con dimensioni normali](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](filling-a-list-using-cascadingdropdown-vb.md)
> [Successivo](presetting-list-entries-with-cascadingdropdown-vb.md)
