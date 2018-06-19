---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Utilizza CascadingDropDown con un Database (VB) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4fd5d2b28540f6e2f751da9fb9d0df5f9995c20b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873586"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a><span data-ttu-id="8aeb0-103">Utilizza CascadingDropDown con un Database (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="8aeb0-103">Using CascadingDropDown with a Database (VB)</span></span>
====================
<span data-ttu-id="8aeb0-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8aeb0-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8aeb0-105">[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8aeb0-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span></span>

> <span data-ttu-id="8aeb0-106">Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8aeb0-107">Affinché il funzionamento, è necessario creare un servizio web speciale.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="8aeb0-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="8aeb0-108">Overview</span></span>

<span data-ttu-id="8aeb0-109">Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8aeb0-110">(Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Affinché il funzionamento, è necessario creare un servizio web speciale.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="8aeb0-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="8aeb0-111">Steps</span></span>

<span data-ttu-id="8aeb0-112">Prima di tutto, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-112">First of all, a data source is required.</span></span> <span data-ttu-id="8aeb0-113">Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="8aeb0-114">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="8aeb0-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="8aeb0-115">Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="8aeb0-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="8aeb0-116">Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="8aeb0-117">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="8aeb0-118">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="8aeb0-119">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il &lt; `form` &gt; elemento):</span><span class="sxs-lookup"><span data-stu-id="8aeb0-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

<span data-ttu-id="8aeb0-120">Nel passaggio successivo, sono necessari due controlli DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="8aeb0-121">In questo esempio, viene usato il fornitore e informazioni di contatto da AdventureWorks, pertanto è creare un unico elenco per i fornitori disponibili e uno per i contatti disponibili:</span><span class="sxs-lookup"><span data-stu-id="8aeb0-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

<span data-ttu-id="8aeb0-122">Quindi, è necessario aggiungere due Extender CascadingDropDown alla pagina.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="8aeb0-123">Uno compilato automaticamente il primo elenco (fornitori), e l'altro nel secondo elenco (contatti).</span><span class="sxs-lookup"><span data-stu-id="8aeb0-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="8aeb0-124">Impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8aeb0-124">The following attributes must be set:</span></span>

- <span data-ttu-id="8aeb0-125">`ServicePath`: URL di un servizio web recapitare le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="8aeb0-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="8aeb0-126">`ServiceMethod`: Metodo web recapito le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="8aeb0-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="8aeb0-127">`TargetControlID`: ID dell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="8aeb0-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="8aeb0-128">`Category`: Informazioni sulle categorie viene inviati al metodo web quando viene chiamato</span><span class="sxs-lookup"><span data-stu-id="8aeb0-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="8aeb0-129">`PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati elenco dal server</span><span class="sxs-lookup"><span data-stu-id="8aeb0-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="8aeb0-130">`ParentControlID`: (facoltativo) che il caricamento di trigger dell'elenco corrente di elenco a discesa di padre</span><span class="sxs-lookup"><span data-stu-id="8aeb0-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="8aeb0-131">A seconda del linguaggio di programmazione utilizzato, viene modificato il nome del servizio web in questione, ma tutti gli altri valori di attributo sono uguali.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="8aeb0-132">Di seguito è l'elemento CascadingDropDown per il primo elenco a discesa:</span><span class="sxs-lookup"><span data-stu-id="8aeb0-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

<span data-ttu-id="8aeb0-133">È necessario impostare le estensioni di controllo per l'elenco secondo il `ParentControlID` attributo in modo che la selezione di una voce nei trigger elenco fornitori durante il caricamento associati a elementi dell'elenco di contatti.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

<span data-ttu-id="8aeb0-134">Il lavoro effettivo viene quindi eseguito nel servizio web, che è impostato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="8aeb0-135">Si noti che il `[ScriptService]` attributo viene utilizzato, in caso contrario AJAX ASP.NET non è possibile creare il proxy JavaScript per accedere ai metodi web dal codice di script sul lato client.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

<span data-ttu-id="8aeb0-136">La firma dei metodi web chiamato da CascadingDropDown è come segue:</span><span class="sxs-lookup"><span data-stu-id="8aeb0-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

<span data-ttu-id="8aeb0-137">Pertanto il valore restituito deve essere una matrice di tipo `CascadingDropDownNameValue` definito da Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="8aeb0-138">Il `GetVendors()` metodo è piuttosto semplice da implementare: il codice si connette al database AdventureWorks e i primi 25 fornitori una query.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="8aeb0-139">Il primo parametro di `CascadingDropDownNameValue` costruttore è la didascalia della voce di elenco, il secondo valore (attributo value in del formato HTML &lt; `option` &gt; elemento).</span><span class="sxs-lookup"><span data-stu-id="8aeb0-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="8aeb0-140">Ecco il codice:</span><span class="sxs-lookup"><span data-stu-id="8aeb0-140">Here is the code:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

<span data-ttu-id="8aeb0-141">Ottenere i contatti associati per un fornitore (nome del metodo: `GetContactsForVendor()`) è leggermente più complessi.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="8aeb0-142">Prima di tutto, è necessario determinare il fornitore a cui è stato selezionato nel primo elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="8aeb0-143">Control Toolkit definisce un metodo di supporto per l'attività: la `ParseKnownCategoryValuesString()` metodo restituisce un `StringDictionary` elemento con i dati dell'elenco a discesa:</span><span class="sxs-lookup"><span data-stu-id="8aeb0-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

<span data-ttu-id="8aeb0-144">Per motivi di sicurezza, questi dati devono essere convalidati prima.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="8aeb0-145">Pertanto, se è presente una voce del fornitore (perché il `Category` del primo elemento CascadingDropDown è impostata su `"Vendor"`), è possibile recuperare l'ID del fornitore selezionato:</span><span class="sxs-lookup"><span data-stu-id="8aeb0-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

<span data-ttu-id="8aeb0-146">Il resto del metodo è piuttosto semplice, quindi.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="8aeb0-147">ID del fornitore viene utilizzato come parametro per una query SQL che recupera tutti i contatti associati di tale fornitore.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="8aeb0-148">In questo caso, il metodo restituisce una matrice di tipo `CascadingDropDownNameValue`.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

<span data-ttu-id="8aeb0-149">Caricare la pagina ASP.NET e, dopo un breve periodo di tempo nell'elenco di fornitori viene compilato con le voci di 25.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="8aeb0-150">Selezionare una voce e notare come secondo elenco a discesa viene riempita con i dati.</span><span class="sxs-lookup"><span data-stu-id="8aeb0-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="8aeb0-151">[![Nel primo elenco viene compilato automaticamente](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8aeb0-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)</span></span>

<span data-ttu-id="8aeb0-152">Nel primo elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine ingrandita](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8aeb0-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span></span>


<span data-ttu-id="8aeb0-153">[![Nel secondo elenco viene compilato in base della selezione nel primo elenco](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8aeb0-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)</span></span>

<span data-ttu-id="8aeb0-154">Nel secondo elenco viene compilato in base alla selezione nel primo elenco ([fare clic per visualizzare l'immagine ingrandita](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8aeb0-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8aeb0-155">[Precedente](filling-a-list-using-cascadingdropdown-vb.md)
> [Successivo](presetting-list-entries-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8aeb0-155">[Previous](filling-a-list-using-cascadingdropdown-vb.md)
[Next](presetting-list-entries-with-cascadingdropdown-vb.md)</span></span>
