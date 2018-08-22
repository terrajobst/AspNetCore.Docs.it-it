---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Appartenenza ASP.NET | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 6 aggiunge le appartenenze di ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: c847db058bc03115210f12eeb0c3c76fecc8a31e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834312"
---
<a name="part-6-aspnet-membership"></a>Parte 6: Appartenenza ASP.NET
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 6 aggiunge le appartenenze di ASP.NET.


## <a id="_Toc260221672"></a>  Utilizzo di appartenenza ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Fare clic su sicurezza

![](tailspin-spyworks-part-6/_static/image1.jpg)

Assicurarsi che si sta usando l'autenticazione basata su form.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Usare il collegamento "Create User" per creare un paio di utenti.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Al termine, fare riferimento alla finestra Esplora soluzioni e aggiornare la visualizzazione.

![](tailspin-spyworks-part-6/_static/image2.png)

Si noti che il file ASPNETDB. È stato creato correttamente MDF. Questo file contiene le tabelle per supportare i servizi ASP.NET core, ad esempio l'appartenenza.

A questo punto è possibile iniziare a implementare il processo di estrazione.

Iniziare creando una pagina CheckOut.aspx.

La pagina CheckOut.aspx solo deve essere disponibile per gli utenti che sono registrati in modo che Microsoft limiterà l'accesso a effettuato l'accesso degli utenti e reindirizzerà gli utenti che non sono connessi alla pagina di accesso.

A questo scopo si aggiungerà quanto segue alla sezione di configurazione del file Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Il modello per le applicazioni Web Form ASP.NET automaticamente aggiunta una sezione di autenticazione al file Web. config e stabilito la pagina di accesso predefinito.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

È necessario modificare il codice aspx file per eseguire la migrazione di un carrello acquisti anonimo quando l'utente accede code-behind. La pagina Cambia\_evento di caricamento come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Aggiungere quindi un gestore dell'evento "LoggedIn" simile al seguente per impostare il nome della sessione per l'utente appena connesso e modificare l'id sessione temporaneo nel carrello acquisti in quello dell'utente chiamando il metodo MigrateCart nella nostra classe MyShoppingCart. (Implementata nel file con estensione cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementare il metodo MigrateCart() simile al seguente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

In checkout.aspx useremo un controllo EntityDataSource e un controllo GridView in, vedere la pagina così come è stato fatto nella nostra pagina carrello acquisti.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Si noti che il controllo GridView specifica un gestore di evento "ondatabound" denominato MyList\_RowDataBound così è possibile implementare tale gestore eventi simile al seguente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Questo metodo modo, viene mantenuto un totale parziale del carrello come ogni riga è associata e aggiorni la riga nella parte inferiore di GridView.

A questo punto sono state implementate una presentazione di "recensione" dell'ordine da inserire.

È possibile gestire un scenario del carrello vuoto mediante l'aggiunta di poche righe di codice alla nostra pagina\_evento di caricamento:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Quando l'utente fa clic sul pulsante "Invia" si eseguirà il codice seguente nel gestore dell'evento fare clic sul pulsante Submit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

La sostanza"" del processo di invio dell'ordine è per essere implementata nel metodo SubmitOrder() della nostra classe MyShoppingCart.

SubmitOrder sarà:

- Eseguire tutte le voci nel carrello acquisti e usarli per creare un nuovo Record di ordine e i record OrderDetails associati.
- Calcola la data di spedizione.
- Deselezionare il carrello acquisti.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Ai fini di questa applicazione di esempio sarà calcoliamo una data di spedizione semplicemente aggiungendo due giorni alla data corrente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Esecuzione dell'applicazione ora consentirà per testare il processo di acquisto dall'inizio alla fine.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-5.md)
> [Successivo](tailspin-spyworks-part-7.md)
