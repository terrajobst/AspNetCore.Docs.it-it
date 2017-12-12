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
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: Pagine finale, gestione delle eccezioni e conclusione
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 8 aggiunge una pagina di contatto, sulla pagina e la gestione delle eccezioni. Questa è la conclusione della serie.


## <a id="_Toc260221680"></a>Contattare pagina (invio messaggio di posta elettronica da ASP.NET)

Creare una nuova pagina denominata ContactUs.aspx

Utilizzando la finestra di progettazione, creare il formato seguente, preso nota speciale per includere il ToolkitScriptManager e il controllo dell'Editor dal AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Fare doppio clic sul pulsante "Invia" per generare un gestore eventi click nel file code-behind e implementare un metodo per inviare le informazioni di contatto come un messaggio di posta elettronica.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Questo codice si presuppone che il file Web. config contiene una voce nella sezione di configurazione che specifica il server SMTP da utilizzare per l'invio di posta elettronica.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Informazioni sulla pagina

Creare una pagina denominata AboutUs. aspx e aggiungere il contenuto desiderato.

## <a id="_Toc260221682"></a>Gestore di eccezioni globale

Infine, in tutta l'applicazione è stata generata un'eccezione di eccezioni ed esistono circostanze impreviste che fredde anche eccezioni causa non gestita nell'applicazione web.

Non si desidera mai un'eccezione non gestita da visualizzare per un visitatore di un sito web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Oltre ad avere un'esperienza utente terribili eccezioni non gestite possono essere anche un problema di sicurezza.

Per risolvere questo problema verrà implementato un gestore eccezioni globale.

A tale scopo, aprire il file Global. asax e annotare il seguente gestore eventi generati in precedenza.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Aggiungere il codice per implementare l'applicazione\_gestore degli errori come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Quindi aggiungere una pagina denominata aspx alla soluzione e aggiungere questo frammento di markup.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Ora nella pagina\_caricare estrazione del gestore eventi i messaggi di errore dall'oggetto Request.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Conclusione

Abbiamo visto che tale Web Form ASP.NET rende più semplice per creare un sito Web complesse con accesso al database, l'appartenenza, AJAX, e così via. abbastanza rapidamente.

Probabilmente questa esercitazione è state ricevute gli strumenti che necessari per iniziare la creazione di applicazioni personalizzate Web Form ASP.NET!

>[!div class="step-by-step"]
[Precedente](tailspin-spyworks-part-7.md)
