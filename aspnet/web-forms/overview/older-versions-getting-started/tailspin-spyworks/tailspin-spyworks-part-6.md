---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: "Parte 6: L'appartenenza ASP.NET | Documenti Microsoft"
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 6 aggiunge le appartenenze di ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a>Parte 6: L'appartenenza ASP.NET
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 6 aggiunge le appartenenze di ASP.NET.


## <a id="_Toc260221672"></a>  Utilizzo di appartenenze ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Fare clic su protezione

![](tailspin-spyworks-part-6/_static/image1.jpg)

Assicurarsi che si sta usando l'autenticazione basata su form.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Utilizzare il collegamento "Create User" per creare un paio di utenti.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Al termine, fare riferimento alla finestra di Esplora soluzioni e aggiornare la visualizzazione.

![](tailspin-spyworks-part-6/_static/image2.png)

Si noti che il file ASPNETDB. Fine file MDF è stato creato. Questo file contiene le tabelle per supportare i servizi ASP.NET di base come l'appartenenza.

Ora è possibile iniziare l'implementazione del processo di estrazione.

Iniziare creando una pagina CheckOut.aspx.

La pagina CheckOut.aspx solo deve essere disponibile per gli utenti connessi in modo si verrà limitato l'accesso ai registrato in utenti e reindirizzerà gli utenti che non sono connessi alla pagina di accesso.

A tale scopo seguente verrà aggiunto alla sezione di configurazione del file Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Il modello per le applicazioni Web Form ASP.NET viene automaticamente aggiunta una sezione di autenticazione per il file Web. config e stabilita pagina di accesso.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

È necessario modificare il login file code-behind per eseguire la migrazione di un carrello acquisti anonimo quando l'utente effettua l'accesso. Modificare la pagina\_evento di caricamento come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Quindi aggiungere un gestore dell'evento simile al seguente per impostare il nome della sessione per l'utente appena connesso e modificare l'id sessione temporaneo nel carrello acquisti a quella dell'utente chiamando il metodo MigrateCart della classe MyShoppingCart "LoggedIn". (Implementato nel file con estensione cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementare il metodo MigrateCart() come segue.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

In checkout.aspx utilizzeremo un controllo EntityDataSource e un controllo GridView in, vedere la pagina quanto abbiamo nella nostra pagina carrello acquisti.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Si noti che il controllo GridView specifica un gestore di evento "ondatabound" denominato MyList\_RowDataBound questo punto implementare il gestore dell'evento simile al seguente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Mantenere questo metodo un totale parziale del Market carrello come ogni riga è associata e aggiorna la riga inferiore di GridView.

In questa fase è stata implementata una presentazione di "verifica" nell'ordine da inserire.

Verrà gestito un scenario del carrello vuoto mediante l'aggiunta di alcune righe di codice alla pagina dei\_evento di caricamento:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Quando l'utente fa clic sul pulsante "Invia" eseguiamo il codice seguente nel gestore di evento di fare clic su pulsante Submit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"Carne" del processo di invio dell'ordine è per essere implementato nel metodo della classe organizzazione MyShoppingCart SubmitOrder().

SubmitOrder sarà:

- Eseguire tutti gli articoli nel carrello acquisti e utilizzarli per creare un nuovo Record di ordine e i record OrderDetails associati.
- Calcolare la data di spedizione.
- Deselezionare il carrello acquisti.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Ai fini di questa applicazione di esempio è sarà calcolare una data di spedizione semplicemente aggiungendo due giorni alla data corrente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Esegue l'applicazione ora consentirà per testare il processo di acquisto dall'inizio alla fine.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-5.md)
> [Successivo](tailspin-spyworks-part-7.md)
