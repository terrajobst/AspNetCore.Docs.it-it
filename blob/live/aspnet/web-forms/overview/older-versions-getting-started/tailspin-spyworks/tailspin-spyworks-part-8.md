---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Pagine finale, gestione delle eccezioni e conclusione | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 8 aggiunge una pagina di contatto, sulla pagina e l'eccezione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0dd1717ff1051f18a78fe77402c7603008b9b486
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="8e5f3-104">Parte 8: Pagine finale, gestione delle eccezioni e conclusione</span><span class="sxs-lookup"><span data-stu-id="8e5f3-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="8e5f3-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="8e5f3-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="8e5f3-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="8e5f3-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="8e5f3-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="8e5f3-109">Parte 8 aggiunge una pagina di contatto, sulla pagina e la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="8e5f3-110">Questa è la conclusione della serie.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a><span data-ttu-id="8e5f3-111">Contattare pagina (invio messaggio di posta elettronica da ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="8e5f3-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="8e5f3-112">Creare una nuova pagina denominata ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="8e5f3-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="8e5f3-113">Utilizzando la finestra di progettazione, creare il formato seguente, preso nota speciale per includere il ToolkitScriptManager e il controllo dell'Editor dal AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="8e5f3-114">.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="8e5f3-115">Fare doppio clic sul pulsante "Invia" per generare un gestore eventi click nel file code-behind e implementare un metodo per inviare le informazioni di contatto come un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="8e5f3-116">Questo codice si presuppone che il file Web. config contiene una voce nella sezione di configurazione che specifica il server SMTP da utilizzare per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="8e5f3-117">Informazioni sulla pagina</span><span class="sxs-lookup"><span data-stu-id="8e5f3-117">About Page</span></span>

<span data-ttu-id="8e5f3-118">Creare una pagina denominata AboutUs. aspx e aggiungere il contenuto desiderato.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="8e5f3-119">Gestore di eccezioni globale</span><span class="sxs-lookup"><span data-stu-id="8e5f3-119">Global Exception Handler</span></span>

<span data-ttu-id="8e5f3-120">Infine, in tutta l'applicazione è stata generata un'eccezione di eccezioni ed esistono circostanze impreviste che fredde anche eccezioni causa non gestita nell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="8e5f3-121">Non si desidera mai un'eccezione non gestita da visualizzare per un visitatore di un sito web.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="8e5f3-122">Oltre ad avere un'esperienza utente terribili eccezioni non gestite possono essere anche un problema di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="8e5f3-123">Per risolvere questo problema verrà implementato un gestore eccezioni globale.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="8e5f3-124">A tale scopo, aprire il file Global. asax e annotare il seguente gestore eventi generati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="8e5f3-125">Aggiungere il codice per implementare l'applicazione\_gestore degli errori come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="8e5f3-126">Quindi aggiungere una pagina denominata aspx alla soluzione e aggiungere questo frammento di markup.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="8e5f3-127">Ora nella pagina\_caricare estrazione del gestore eventi i messaggi di errore dall'oggetto Request.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="8e5f3-128">Conclusione</span><span class="sxs-lookup"><span data-stu-id="8e5f3-128">Conclusion</span></span>

<span data-ttu-id="8e5f3-129">Abbiamo visto che tale Web Form ASP.NET rende più semplice per creare un sito Web complesse con accesso al database, l'appartenenza, AJAX, e così via.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-129">We've seen that that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="8e5f3-130">abbastanza rapidamente.</span><span class="sxs-lookup"><span data-stu-id="8e5f3-130">pretty quickly.</span></span>

<span data-ttu-id="8e5f3-131">Probabilmente questa esercitazione è state ricevute gli strumenti che necessari per iniziare la creazione di applicazioni personalizzate Web Form ASP.NET!</span><span class="sxs-lookup"><span data-stu-id="8e5f3-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8e5f3-132">Precedente</span><span class="sxs-lookup"><span data-stu-id="8e5f3-132">Previous</span></span>](tailspin-spyworks-part-7.md)
