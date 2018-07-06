---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Creazione di un AJAX personalizzato controllo controllo Extender Toolkit (VB) | Microsoft Docs
author: microsoft
description: Estensioni personalizzate consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza la necessità di creare nuove classi.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: a996409e8e9d37806fcbee8b7cc1c509dd9700fa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394547"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a><span data-ttu-id="34ffb-103">Creazione di un controllo Toolkit controllo Extender di AJAX personalizzato (VB)</span><span class="sxs-lookup"><span data-stu-id="34ffb-103">Creating a Custom AJAX Control Toolkit Control Extender (VB)</span></span>
====================
<span data-ttu-id="34ffb-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="34ffb-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="34ffb-105">Estensioni personalizzate consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza la necessità di creare nuove classi.</span><span class="sxs-lookup"><span data-stu-id="34ffb-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="34ffb-106">In questa esercitazione descrive come creare un controllo extender di AJAX Control Toolkit personalizzato.</span><span class="sxs-lookup"><span data-stu-id="34ffb-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="34ffb-107">Creiamo un semplice, ma utile, nuova estensione che lo stato di un pulsante cambia da disabilitato ad abilitato quando si digita il testo in una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="34ffb-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="34ffb-108">Dopo aver letto questa esercitazione, sarà possibile estendere ASP.NET AJAX Toolkit con i propri dispositivi Extender dei controlli.</span><span class="sxs-lookup"><span data-stu-id="34ffb-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="34ffb-109">È possibile creare dispositivi Extender dei controlli personalizzati utilizzando Visual Studio o Visual Web Developer (assicurarsi di avere la versione più recente di Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="34ffb-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="34ffb-110">Panoramica dell'oggetto Extender DisabledButton</span><span class="sxs-lookup"><span data-stu-id="34ffb-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="34ffb-111">Il nuovo controllo extender viene denominato il dispositivo extender DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="34ffb-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="34ffb-112">Questo dispositivo extender hanno tre proprietà:</span><span class="sxs-lookup"><span data-stu-id="34ffb-112">This extender will have three properties:</span></span>

- <span data-ttu-id="34ffb-113">TargetControlID - casella di testo che si estende il controllo.</span><span class="sxs-lookup"><span data-stu-id="34ffb-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="34ffb-114">TargetButtonIID - pulsante che è abilitato o disabilitato.</span><span class="sxs-lookup"><span data-stu-id="34ffb-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="34ffb-115">DisabledText - il testo che viene inizialmente visualizzato nel pulsante.</span><span class="sxs-lookup"><span data-stu-id="34ffb-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="34ffb-116">Quando si inizia a digitare, il pulsante Visualizza il valore della proprietà testo del pulsante.</span><span class="sxs-lookup"><span data-stu-id="34ffb-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="34ffb-117">Associare il dispositivo extender DisabledButton a un controllo TextBox e Button.</span><span class="sxs-lookup"><span data-stu-id="34ffb-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="34ffb-118">Prima di digitare un testo, il pulsante è disabilitato e la casella di testo e un pulsante simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="34ffb-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

<span data-ttu-id="34ffb-119">([Fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span></span>


<span data-ttu-id="34ffb-120">Dopo avere avviato la digitazione di testo, il pulsante è abilitato e la casella di testo e un pulsante simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="34ffb-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

<span data-ttu-id="34ffb-121">([Fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="34ffb-122">Per creare l'estensione del controllo, è necessario creare i tre file seguenti:</span><span class="sxs-lookup"><span data-stu-id="34ffb-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="34ffb-123">DisabledButtonExtender.vb - questo file è la classe di controllo lato server che gestisce la creazione dell'extender e consentono di impostare le proprietà in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="34ffb-123">DisabledButtonExtender.vb - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="34ffb-124">Definisce anche le proprietà che possono essere impostate sul dispositivo extender.</span><span class="sxs-lookup"><span data-stu-id="34ffb-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="34ffb-125">Queste proprietà sono accessibili tramite il codice e in fase di progettazione e corrispondano alle proprietà definite nel file DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="34ffb-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="34ffb-126">DisabledButtonBehavior.js: Questo file è dove si aggiungeranno tutta la logica di script client.</span><span class="sxs-lookup"><span data-stu-id="34ffb-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="34ffb-127">DisabledButtonDesigner.vb - questa classe abilita la funzionalità in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="34ffb-127">DisabledButtonDesigner.vb - This class enables design-time functionality.</span></span> <span data-ttu-id="34ffb-128">Questa classe è necessario se si desidera che il controllo extender per funzionare correttamente con progettazione di Visual Studio e Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="34ffb-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="34ffb-129">Pertanto, un controllo extender è costituito da un controllo lato server, un comportamento del lato client e una classe della finestra di progettazione sul lato server.</span><span class="sxs-lookup"><span data-stu-id="34ffb-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="34ffb-130">Informazioni su come creare tutti e tre i file nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="34ffb-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="34ffb-131">Creare il progetto e il sito Web di estensione personalizzato</span><span class="sxs-lookup"><span data-stu-id="34ffb-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="34ffb-132">Il primo passaggio consiste nel creare un progetto libreria di classi e il sito Web in Visual Studio e Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="34ffb-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="34ffb-133">Abbiamo ll creare il dispositivo extender personalizzato nel progetto libreria di classi e testare il dispositivo extender personalizzato nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="34ffb-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="34ffb-134">Let s iniziano con il sito Web.</span><span class="sxs-lookup"><span data-stu-id="34ffb-134">Let�s start with the website.</span></span> <span data-ttu-id="34ffb-135">Seguire questi passaggi per creare il sito Web:</span><span class="sxs-lookup"><span data-stu-id="34ffb-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="34ffb-136">Selezionare l'opzione di menu **File, nuovo sito Web**.</span><span class="sxs-lookup"><span data-stu-id="34ffb-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="34ffb-137">Selezionare il **sito Web ASP.NET** modello.</span><span class="sxs-lookup"><span data-stu-id="34ffb-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="34ffb-138">Denominare il nuovo sito Web *Website1*.</span><span class="sxs-lookup"><span data-stu-id="34ffb-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="34ffb-139">Scegliere il **OK** pulsante.</span><span class="sxs-lookup"><span data-stu-id="34ffb-139">Click the **OK** button.</span></span>

<span data-ttu-id="34ffb-140">Successivamente, è necessario creare il progetto di libreria di classi che conterrà il codice per il controllo extender:</span><span class="sxs-lookup"><span data-stu-id="34ffb-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="34ffb-141">Selezionare l'opzione di menu **nuovo progetto di File, Add,**.</span><span class="sxs-lookup"><span data-stu-id="34ffb-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="34ffb-142">Selezionare il **libreria di classi** modello.</span><span class="sxs-lookup"><span data-stu-id="34ffb-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="34ffb-143">Denominare la nuova libreria di classi con il nome **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="34ffb-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="34ffb-144">Scegliere il **OK** pulsante.</span><span class="sxs-lookup"><span data-stu-id="34ffb-144">Click the **OK** button.</span></span>

<span data-ttu-id="34ffb-145">Dopo aver completato questi passaggi, la finestra Esplora soluzioni dovrebbe essere simile nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="34ffb-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="34ffb-146">[![Soluzione con progetto di libreria di classi e il sito Web](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="34ffb-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="34ffb-147">**Figura 01**: soluzione con progetto di libreria di classi e il sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span></span>


<span data-ttu-id="34ffb-148">Successivamente, è necessario aggiungere tutti i riferimenti all'assembly necessari per il progetto di libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="34ffb-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="34ffb-149">Fare clic sul progetto CustomExtenders e selezionare l'opzione di menu **Aggiungi riferimento**.</span><span class="sxs-lookup"><span data-stu-id="34ffb-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="34ffb-150">Selezionare la scheda .NET.</span><span class="sxs-lookup"><span data-stu-id="34ffb-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="34ffb-151">Aggiungere riferimenti agli assembly riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="34ffb-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="34ffb-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="34ffb-152">System.Web.dll</span></span>
    2. <span data-ttu-id="34ffb-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="34ffb-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="34ffb-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="34ffb-154">System.Design.dll</span></span>
    4. <span data-ttu-id="34ffb-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="34ffb-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="34ffb-156">Selezionare la scheda Sfoglia.</span><span class="sxs-lookup"><span data-stu-id="34ffb-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="34ffb-157">Aggiungere un riferimento all'assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="34ffb-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="34ffb-158">Questo assembly si trova nella cartella in cui è stato scaricato AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="34ffb-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="34ffb-159">È possibile verificare che sia stato aggiunto tutti i riferimenti a destra il pulsante destro del progetto, selezionando le proprietà e facendo clic sulla scheda Riferimenti (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="34ffb-159">You can verify that you have added all of the right references by right-clicking your project, selecting Properties, and clicking the References tab (see Figure 2).</span></span>


<span data-ttu-id="34ffb-160">[![Cartella riferimenti con i riferimenti necessari](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="34ffb-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span></span>

<span data-ttu-id="34ffb-161">**Figura 02**: cartella riferimenti con i riferimenti necessari ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="34ffb-162">Creazione di estensione del controllo personalizzato</span><span class="sxs-lookup"><span data-stu-id="34ffb-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="34ffb-163">Ora che abbiamo la nostra libreria di classi, è possibile iniziare a creare il controllo extender.</span><span class="sxs-lookup"><span data-stu-id="34ffb-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="34ffb-164">Let s iniziano con la vasta gamma di una classe del controllo extender personalizzato (vedere l'elenco 1).</span><span class="sxs-lookup"><span data-stu-id="34ffb-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="34ffb-165">**Listato 1 - MyCustomExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="34ffb-165">**Listing 1 - MyCustomExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

<span data-ttu-id="34ffb-166">Esistono diversi aspetti che noterete sulla classe del controllo extender nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="34ffb-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="34ffb-167">In primo luogo, si noti che la classe eredita dalla classe di base ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="34ffb-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="34ffb-168">Tutti i controlli extender di AJAX Control Toolkit derivano da questa classe di base.</span><span class="sxs-lookup"><span data-stu-id="34ffb-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="34ffb-169">Ad esempio, la classe di base include la proprietà ID destinazione che è una proprietà obbligatoria di ogni controllo extender.</span><span class="sxs-lookup"><span data-stu-id="34ffb-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="34ffb-170">Successivamente, si noti che la classe include i seguenti due attributi correlati per lo script client:</span><span class="sxs-lookup"><span data-stu-id="34ffb-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="34ffb-171">WebResource - fa sì che un file da includere come risorsa incorporata in un assembly.</span><span class="sxs-lookup"><span data-stu-id="34ffb-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="34ffb-172">ClientScriptResource - fa sì che una risorsa di script da recuperare da un assembly.</span><span class="sxs-lookup"><span data-stu-id="34ffb-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="34ffb-173">L'attributo WebResource viene utilizzato per incorporare il file MyControlBehavior.js JavaScript nell'assembly quando il dispositivo extender personalizzato viene compilato.</span><span class="sxs-lookup"><span data-stu-id="34ffb-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="34ffb-174">L'attributo ClientScriptResource viene utilizzato per recuperare lo script MyControlBehavior.js dall'assembly quando il dispositivo extender personalizzato viene utilizzato in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="34ffb-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="34ffb-175">Affinché gli attributi WebResource e ClientScriptResource funzionamento, è necessario compilare il file JavaScript come risorsa incorporata.</span><span class="sxs-lookup"><span data-stu-id="34ffb-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="34ffb-176">Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *risorsa incorporata* per il **azione di compilazione** proprietà.</span><span class="sxs-lookup"><span data-stu-id="34ffb-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="34ffb-177">Si noti che il controllo extender include anche un attributo TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="34ffb-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="34ffb-178">Questo attributo viene utilizzato per specificare il tipo di controllo che verrà esteso mediante l'estensione del controllo.</span><span class="sxs-lookup"><span data-stu-id="34ffb-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="34ffb-179">Nel caso listato 1, il controllo extender viene utilizzato per estendere una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="34ffb-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="34ffb-180">Infine, si noti che il dispositivo extender personalizzato include una proprietà denominata MyProperty.</span><span class="sxs-lookup"><span data-stu-id="34ffb-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="34ffb-181">La proprietà è contrassegnata con l'attributo ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="34ffb-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="34ffb-182">I metodi GetPropertyValue() e SetPropertyValue() vengono utilizzati per passare il valore della proprietà dall'estensione del controllo lato server per il comportamento lato client.</span><span class="sxs-lookup"><span data-stu-id="34ffb-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="34ffb-183">Consentire s andare avanti e implementare il codice per il dispositivo extender DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="34ffb-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="34ffb-184">Il codice per questa estensione è reperibile nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="34ffb-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="34ffb-185">**Listato 2 - DisabledButtonExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="34ffb-185">**Listing 2 - DisabledButtonExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

<span data-ttu-id="34ffb-186">Il dispositivo extender DisabledButton nel listato 2 ha due proprietà denominate TargetButtonID e DisabledText.</span><span class="sxs-lookup"><span data-stu-id="34ffb-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="34ffb-187">Il IDReferenceProperty applicato alla proprietà TargetButtonID impedisce solo per l'ID di un controllo pulsante assegnando a questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="34ffb-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="34ffb-188">Gli attributi WebResource e ClientScriptResource associare un comportamento del lato client che si trova in un file denominato DisabledButtonBehavior.js con il dispositivo extender.</span><span class="sxs-lookup"><span data-stu-id="34ffb-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="34ffb-189">Viene illustrato questo file JavaScript nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="34ffb-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="34ffb-190">La creazione del comportamento di controllo Extender personalizzato</span><span class="sxs-lookup"><span data-stu-id="34ffb-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="34ffb-191">Il componente lato client di un controllo extender viene chiamato un comportamento.</span><span class="sxs-lookup"><span data-stu-id="34ffb-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="34ffb-192">La logica effettiva per la disattivazione e il pulsante è contenuta nel comportamento DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="34ffb-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="34ffb-193">Il codice JavaScript per il comportamento è incluso nel listato 3.</span><span class="sxs-lookup"><span data-stu-id="34ffb-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="34ffb-194">**Listato 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="34ffb-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

<span data-ttu-id="34ffb-195">Il file JavaScript nel listato 3 contiene una classe lato client denominata DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="34ffb-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="34ffb-196">Questa classe, ad esempio il gemello sul lato server, include due proprietà denominate TargetButtonID e ottenere DisabledText che è possibile accedere usando\_TargetButtonID/set\_TargetButtonID e Introduzione\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="34ffb-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="34ffb-197">Il metodo Initialize () si associa l'elemento di destinazione per il comportamento di un gestore dell'evento keyup.</span><span class="sxs-lookup"><span data-stu-id="34ffb-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="34ffb-198">Ogni volta che si digita una lettera nella casella di testo associato a questo comportamento, esegue il gestore dell'evento keyup.</span><span class="sxs-lookup"><span data-stu-id="34ffb-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="34ffb-199">Il gestore dell'evento keyup Abilita o disabilita il pulsante a seconda che la casella di testo associata al comportamento contenga qualsiasi testo.</span><span class="sxs-lookup"><span data-stu-id="34ffb-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="34ffb-200">Tenere presente che è necessario compilare il file JavaScript nel listato 3 come risorsa incorporata.</span><span class="sxs-lookup"><span data-stu-id="34ffb-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="34ffb-201">Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *risorsa incorporata* per il **azione di compilazione** proprietà (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="34ffb-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="34ffb-202">Questa opzione è disponibile in Visual Studio e Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="34ffb-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="34ffb-203">[![Aggiunta di un file JavaScript come risorsa incorporata](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="34ffb-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span></span>

<span data-ttu-id="34ffb-204">**Figura 03**: aggiunta di un file JavaScript come risorsa incorporata ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="34ffb-205">Creazione della finestra di progettazione di estensione personalizzato</span><span class="sxs-lookup"><span data-stu-id="34ffb-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="34ffb-206">C'è una classe ultimo che è necessario creare per completare l'oggetto extender.</span><span class="sxs-lookup"><span data-stu-id="34ffb-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="34ffb-207">È necessario creare la classe della finestra di progettazione contenuta nel listato 4.</span><span class="sxs-lookup"><span data-stu-id="34ffb-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="34ffb-208">Questa classe è necessario per rendere il dispositivo extender si comportano in modo corretto con progettazione di Visual Studio e Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="34ffb-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="34ffb-209">**Listato 4 - DisabledButtonDesigner.vb**</span><span class="sxs-lookup"><span data-stu-id="34ffb-209">**Listing 4 - DisabledButtonDesigner.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

<span data-ttu-id="34ffb-210">La finestra di progettazione contenuta nel listato 4 associare il dispositivo extender DisabledButton con l'attributo Designer. È necessario applicare l'attributo di progettazione per la classe DisabledButtonExtender simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="34ffb-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="34ffb-211">Utilizza l'Extender personalizzato</span><span class="sxs-lookup"><span data-stu-id="34ffb-211">Using the Custom Extender</span></span>

<span data-ttu-id="34ffb-212">Ora che è stata terminata la creazione il controllo extender DisabledButton, è possibile usarlo nel nostro sito Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34ffb-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="34ffb-213">In primo luogo, è necessario aggiungere il dispositivo extender personalizzato nella casella degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="34ffb-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="34ffb-214">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="34ffb-214">Follow these steps:</span></span>

1. <span data-ttu-id="34ffb-215">Facendo doppio clic su pagina nella finestra Esplora soluzioni, aprire una pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34ffb-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="34ffb-216">Fare doppio clic su casella degli strumenti e selezionare l'opzione di menu **Scegli elementi**.</span><span class="sxs-lookup"><span data-stu-id="34ffb-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="34ffb-217">Nella finestra di dialogo Scegli elementi della casella degli strumenti, selezionare l'assembly CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="34ffb-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="34ffb-218">Scegliere il **OK** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="34ffb-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="34ffb-219">Dopo aver completato questi passaggi, il controllo extender DisabledButton deve essere visualizzato nella casella degli strumenti (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="34ffb-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="34ffb-220">[![DisabledButton della casella degli strumenti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="34ffb-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span></span>

<span data-ttu-id="34ffb-221">**Figura 04**: DisabledButton della casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span></span>


<span data-ttu-id="34ffb-222">Successivamente, è necessario creare una nuova pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34ffb-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="34ffb-223">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="34ffb-223">Follow these steps:</span></span>

1. <span data-ttu-id="34ffb-224">Creare una nuova pagina ASP.NET denominata ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="34ffb-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="34ffb-225">Trascinare un controllo ScriptManager nella pagina.</span><span class="sxs-lookup"><span data-stu-id="34ffb-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="34ffb-226">Trascinare un controllo TextBox nella pagina.</span><span class="sxs-lookup"><span data-stu-id="34ffb-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="34ffb-227">Trascinare un controllo pulsante della pagina.</span><span class="sxs-lookup"><span data-stu-id="34ffb-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="34ffb-228">Nella finestra Proprietà modificare la proprietà ID del pulsante sul valore <em>btnSave</em> e la proprietà Text sul valore *salvare\**.</span><span class="sxs-lookup"><span data-stu-id="34ffb-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="34ffb-229">Viene creata una pagina con un controllo ASP.NET TextBox e Button standard.</span><span class="sxs-lookup"><span data-stu-id="34ffb-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="34ffb-230">Successivamente, è necessario estendere il controllo casella di testo con il dispositivo extender DisabledButton:</span><span class="sxs-lookup"><span data-stu-id="34ffb-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="34ffb-231">Selezionare il **Aggiungi estensione** opzione per aprire la finestra di dialogo Creazione guidata attività (vedere la figura 5).</span><span class="sxs-lookup"><span data-stu-id="34ffb-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="34ffb-232">Si noti che la finestra di dialogo include l'estensione DisabledButton personalizzato.</span><span class="sxs-lookup"><span data-stu-id="34ffb-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="34ffb-233">Selezionare il dispositivo extender DisabledButton e scegliere il **OK** pulsante.</span><span class="sxs-lookup"><span data-stu-id="34ffb-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="34ffb-234">[![La finestra di dialogo Creazione guidata dispositivo Extender](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="34ffb-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span></span>

<span data-ttu-id="34ffb-235">**Figura 05**: finestra di dialogo dell'estensione guidata ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span></span>


<span data-ttu-id="34ffb-236">Infine, è possibile impostare le proprietà dell'oggetto extender DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="34ffb-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="34ffb-237">È possibile modificare le proprietà dell'oggetto extender DisabledButton modificando le proprietà del controllo casella di testo:</span><span class="sxs-lookup"><span data-stu-id="34ffb-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="34ffb-238">Selezionare la casella di testo nella finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="34ffb-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="34ffb-239">Nella finestra Proprietà espandere il nodo dispositivi Extender (vedere la figura 6).</span><span class="sxs-lookup"><span data-stu-id="34ffb-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="34ffb-240">Assegnare il valore *salvare* alla proprietà DisabledText e il valore *btnSave* alla proprietà TargetButtonID.</span><span class="sxs-lookup"><span data-stu-id="34ffb-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="34ffb-241">[![Impostazione delle proprietà di estensione](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="34ffb-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span></span>

<span data-ttu-id="34ffb-242">**Figura 06**: l'impostazione delle proprietà di estensione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span></span>


<span data-ttu-id="34ffb-243">Quando si esegue la pagina (premendo F5), il controllo Button è inizialmente disabilitato.</span><span class="sxs-lookup"><span data-stu-id="34ffb-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="34ffb-244">Non appena si inizia a immettere il testo nella casella di testo, il pulsante di controllo è abilitato (vedere la figura 7).</span><span class="sxs-lookup"><span data-stu-id="34ffb-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="34ffb-245">[![Il dispositivo extender DisabledButton in azione](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="34ffb-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span></span>

<span data-ttu-id="34ffb-246">**Figura 07**: DisabledButton il dispositivo extender in azione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="34ffb-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="34ffb-247">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="34ffb-247">Summary</span></span>

<span data-ttu-id="34ffb-248">L'obiettivo di questa esercitazione è stata per illustrare come è possibile estendere AJAX Control Toolkit con controlli extender personalizzato.</span><span class="sxs-lookup"><span data-stu-id="34ffb-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="34ffb-249">In questa esercitazione è stato creato un semplice DisabledButton controllo extender.</span><span class="sxs-lookup"><span data-stu-id="34ffb-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="34ffb-250">Questa estensione è stata implementata tramite la creazione di una classe DisabledButtonExtender, un comportamento DisabledButtonBehavior JavaScript e una classe DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="34ffb-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="34ffb-251">È seguire una serie di passaggi simile ogni volta che si crea un oggetto extender personalizzato.</span><span class="sxs-lookup"><span data-stu-id="34ffb-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="34ffb-252">Precedente</span><span class="sxs-lookup"><span data-stu-id="34ffb-252">Previous</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
