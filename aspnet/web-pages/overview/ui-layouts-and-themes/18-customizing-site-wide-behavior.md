---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizzazione del comportamento a livello di sito per ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: "In questo capitolo viene illustrato come creare le impostazioni per l'intero sito Web o un'intera cartella, anziché una pagina."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: b1caa26a23517bd976addfefac89375ae965eb91
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalizzazione del comportamento a livello di sito per i siti Web ASP.NET (Razor) pagine
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come creare impostazioni sito-side per le pagine in un sito Web ASP.NET Web Pages (Razor).
> 
> Illustra quanto segue:
> 
> - Come eseguire il codice che consente di set di valori (valori globali o le impostazioni di supporto) per tutte le pagine in un sito.
> - Come eseguire il codice che consente di impostare valori per tutte le pagine in una cartella.
> - Come eseguire codice prima e dopo una pagina viene caricata.
> - Come inviare errori a una pagina di errore centrale.
> - Come aggiungere l'autenticazione a tutte le pagine in una cartella.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (pacchetto NuGet)
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 3 e Visual Studio 2013 o Visual Studio Express 2013 per Web, è possibile utilizzare ASP.NET Web Helpers Library.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Aggiungere il codice di avvio del sito Web per ASP.NET Web Pages

Per la maggior parte del codice scritto in ASP.NET Web Pages, una singola pagina può contenere tutto il codice che ha richiesto per la pagina. Ad esempio, se una pagina invia un messaggio di posta elettronica, è possibile inserire tutto il codice per l'operazione in una singola pagina. Ciò può includere il codice per inizializzare le impostazioni per l'invio di posta elettronica (ovvero, per il server SMTP) e per l'invio del messaggio di posta elettronica.

In alcuni casi, è comunque eseguire il codice prima dell'esecuzione di qualsiasi pagina del sito. Ciò è utile per l'impostazione di valori che possono essere utilizzati ovunque nel sito (detto *valori globali*.) Ad esempio, alcuni helper necessario fornire i valori come impostazioni di posta elettronica o le chiavi dell'account. Può essere utile per mantenere queste impostazioni in valori globali.

È possibile farlo creando una pagina denominata  *\_AppStart.cshtml* nella radice del sito. Se questa pagina è presente, viene eseguita la prima volta, verrà richiesta una pagina nel sito. Pertanto, è un ottimo strumento per eseguire il codice per impostare i valori globali. (Poiché  *\_AppStart.cshtml* è un prefisso di un carattere di sottolineatura, non inviare la pagina ASP.NET in un browser anche se gli utenti richiedono direttamente.)

Il diagramma seguente mostra come  *\_AppStart.cshtml* pagina funziona. Quando arriva una richiesta per una pagina e se si tratta la prima richiesta per una pagina nel sito ASP.NET controlla innanzitutto se un  *\_AppStart.cshtml* esistente. In questo caso, qualsiasi codice nel  *\_AppStart.cshtml* pagina viene eseguito e quindi di eseguire la pagina richiesta.

![[immagine]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Impostazione dei valori globali per il sito Web

1. Nella cartella radice di un sito Web di WebMatrix, creare un file denominato  *\_AppStart.cshtml*. Il file deve essere nella radice del sito.
2. Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Questo codice è archiviato un valore nel `AppState` dizionario, che è automaticamente disponibile per tutte le pagine nel sito. Si noti che il  *\_AppStart.cshtml* file non dispone di qualsiasi tipo di markup in essa contenuti. La pagina verrà eseguito il codice e quindi reindirizzare alla pagina richiesta originariamente.

    > [!NOTE]
    > Prestare attenzione quando il codice inserito nel  *\_AppStart.cshtml* file. Se si verificano errori nel codice il  *\_AppStart.cshtml* file, non è possibile avviare il sito Web.
3. Nella cartella radice, creare una nuova pagina denominata *AppName.cshtml*.
4. Sostituire il markup predefinito e il codice con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Questo codice estrae il valore di `AppState` oggetto che viene impostato nel  *\_AppStart.cshtml* pagina.
5. Eseguire il *AppName.cshtml* pagina in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.) La pagina Visualizza il valore globale. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Impostazione dei valori per gli helper

Per un utilizzo ottimo di  *\_AppStart.cshtml* file consiste nell'impostare i valori per gli helper che si utilizzano nel sito e che devono essere inizializzati. Gli esempi tipici sono le impostazioni di posta elettronica per il `WebMail` helper e le chiavi pubbliche e private per il `ReCaptcha` helper. In questi casi è possibile impostare i valori di una volta il  *\_AppStart.cshtml* e quindi si è già impostate per tutte le pagine del sito.

Questa procedura viene illustrato come impostare `WebMail` impostazioni a livello globale. (Per ulteriori informazioni sull'utilizzo di `WebMail` supporto, vedere [aggiunta di posta elettronica a un sito di pagine Web ASP.NET](../getting-started/11-adding-email-to-your-web-site.md).)

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.
2. Se dispone già di un  *\_AppStart.cshtml* file, nella cartella radice di un sito Web, creare un file denominato  *\_AppStart.cshtml*.
3. Aggiungere il seguente `WebMail` le impostazioni per il  *\_AppStart.cshtml* file: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modificare le impostazioni correlate nel codice di posta elettronica seguenti:

    - Impostare `your-SMTP-host` sul nome del server SMTP che è possibile accedere.
    - Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.
    - Impostare `your-account-password` la password per l'account del server SMTP.
    - Impostare `your-email-address-here` sul proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio. (Alcuni provider di posta elettronica non consentono di specificare un altro `From` indirizzi e utilizzerà il nome utente come il `From` indirizzo.)

    Per ulteriori informazioni sulle impostazioni SMTP, vedere [configurazione delle impostazioni di posta elettronica](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) nell'articolo [l'invio di posta elettronica da un sito di pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) e [problemi con l'invio di posta elettronica](https://go.microsoft.com/fwlink/?LinkId=253001#email)nel [pagine Web ASP.NET (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).
- Salvare il  *\_AppStart.cshtml* file e chiuderlo.
- Nella cartella radice di un sito Web, creare nuova pagina denominata *TestEmail.cshtml*.
- Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
- Eseguire il *TestEmail.cshtml* pagina in un browser.
- Compilare i campi per inviare un messaggio di posta elettronica e quindi fare clic su **inviare**.
- Controllare la posta elettronica per verificare che è stato utilizzato il messaggio.

La parte importante di questo esempio è che le impostazioni che in genere non cambiano, ad esempio il nome del server SMTP e le credenziali di posta elettronica, vengono impostate  *\_AppStart.cshtml* file. In questo modo non è necessario impostarle nuovamente in ogni pagina in cui si invia tramite posta elettronica. (Anche se se per qualche motivo, è necessario modificare tali impostazioni, è possibile impostarle singolarmente in una pagina.) Nella pagina, è solo necessario impostare i valori che in genere modificato ogni volta, ad esempio il destinatario e il corpo del messaggio di posta elettronica.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Esecuzione del codice prima e dopo i file in una cartella

Come è possibile utilizzare  *\_AppStart.cshtml* per scrivere il codice prima di eseguire le pagine del sito, è possibile scrivere codice che viene eseguito prima (e, dopo aver) qualsiasi pagina in una cartella particolare di esecuzione. Ciò è utile per operazioni come l'impostazione stessa pagina di layout per tutte le pagine in una cartella o a un controllo che un utente è connesso prima di eseguire una pagina nella cartella.

Per le pagine in particolare le cartelle, è possibile creare codice in un file denominato  *\_Pagestart*. Il diagramma seguente mostra come  *\_Pagestart* pagina funziona. Quando arriva una richiesta per una pagina, ASP.NET esegue la ricerca di un  *\_AppStart.cshtml* pagina e viene eseguito. ASP.NET controlla se è presente un  *\_Pagestart* pagina e in tal caso, viene eseguito. Viene quindi eseguita la pagina richiesta.

All'interno di  *\_Pagestart* pagina, è possibile specificare dove durante l'elaborazione che si desidera che la pagina richiesta per eseguire includendo un `RunPage` metodo. Ciò consente di eseguire codice prima che venga eseguita la pagina richiesta e quindi nuovamente dopo di esso. Se non si include `RunPage`, tutto il codice in  *\_Pagestart* viene eseguito, quindi selezionare la pagina richiesta viene eseguita automaticamente.

![[immagine]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET consente di creare una gerarchia di  *\_Pagestart* file. È possibile inserire un  *\_Pagestart* file nella radice del sito e in qualsiasi sottocartella. Quando viene richiesta una pagina, il  *\_Pagestart* l'esecuzione del livello più alto (più vicino alla radice del sito), seguito dal file di  *\_Pagestart* file nella prossima sottocartella, e così via lungo la struttura sottocartella fino a quando la richiesta raggiunge la cartella che contiene la pagina richiesta. Dopo aver tutti l'applicabili  *\_Pagestart* file è sono eseguito, verrà eseguita la pagina richiesta.

Ad esempio, potrebbe essere la seguente combinazione di  *\_Pagestart* file e *cshtml* file:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Quando si esegue */myfolder/default.cshtml*, si noterà quanto segue:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Esecuzione del codice di inizializzazione per tutte le pagine in una cartella

Per un utilizzo ottimo  *\_Pagestart* file consiste nell'inizializzare la stessa pagina di layout per tutti i file in un'unica cartella.

1. Nella cartella radice, creare una nuova cartella denominata *InitPages*.
2. Nel *InitPages* cartella del sito Web, creare un file denominato  *\_Pagestart* e sostituire il markup predefinito e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Nella radice del sito Web, creare una cartella denominata *Shared*.
4. Nel *Shared* cartella, creare un file denominato  *\_Layout1.cshtml* e sostituire il markup predefinito e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. Nel *InitPages* cartella, creare un file denominato *Content1.cshtml* e sostituire il contenuto esistente con il seguente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. Nel *InitPages* cartella, un file denominato *Content2.cshtml* e sostituire il markup predefinito con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Eseguire *Content1.cshtml* in un browser. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Quando il *Content1.cshtml* pagina viene eseguito, il  *\_Pagestart* file imposta `Layout` e imposta inoltre `PageData["MyBackground"]` su un colore. In *Content1.cshtml*, vengono applicati il layout e il colore.
8. Visualizzazione *Content2.cshtml* in un browser. 

    Il layout è lo stesso, poiché entrambe le pagine utilizzano la stessa pagina layout e colori come inizializzare in  *\_Pagestart*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Utilizzando \_Pagestart per gestire gli errori

Utilizzare un'altra utile per il  *\_Pagestart* file consiste nel creare un modo per gestire gli errori di programmazione (eccezioni) che potrebbero verificarsi in qualsiasi *. cshtml* pagina in una cartella. In questo esempio viene illustrato un modo per eseguire questa operazione.

1. Nella cartella radice, creare una cartella denominata *InitCatch*.
2. Nel *InitCatch* cartella del sito Web, creare un file denominato  *\_Pagestart* e sostituire il codice esistente e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    In questo codice, si tenta di eseguire la pagina richiesta in modo esplicito chiamando il `RunPage` metodo all'interno di un `try` blocco. In caso di errori di programmazione nella richiesta di pagina, il codice all'interno di `catch` blocco viene eseguito. In questo caso, il codice reindirizza a una pagina (*Error.cshtml*) e passa il nome del file che si è verificato l'errore come parte dell'URL. (Creerai la pagina al più presto.)
3. Nel *InitCatch* cartella del sito Web, creare un file denominato *Exception.cshtml* e sostituire il codice esistente e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Ai fini di questo esempio, alle operazioni eseguite in questa pagina è deliberatamente la creazione di un errore durante il tentativo di aprire un file di database che non esiste.
4. Nella cartella radice, creare un file denominato *Error.cshtml* e sostituire il codice esistente e il codice con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    In questa pagina, l'espressione `@Request["source"]` Ottiene il valore non compreso nell'URL e lo visualizza.
5. Nella barra degli strumenti, fare clic su **salvare**.
6. Eseguire *Exception.cshtml* in un browser. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Perché si verifica un errore *Exception.cshtml*,  *\_Pagestart* reindirizza a pagina di *Error.cshtml* file, che viene visualizzato il messaggio.

    Per ulteriori informazioni sulle eccezioni, vedere [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Utilizzando \_Pagestart per limitare l'accesso alla cartella

È inoltre possibile utilizzare il  *\_Pagestart* file per limitare l'accesso a tutti i file in una cartella.

1. In WebMatrix, creare un nuovo sito Web utilizzando il **del sito da un modello** opzione.
2. I modelli disponibili, selezionare **Starter Site**.
3. Nella cartella radice, creare una cartella denominata *AuthenticatedContent*.
4. Nel *AuthenticatedContent* cartella, creare un file denominato  *\_Pagestart* e sostituire il codice esistente e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Il codice avvia, impedendo che tutti i file nella cartella memorizzata nella cache. (Questo è necessario per scenari come i computer pubblici, in cui si desidera pagine memorizzate nella cache di un utente sia disponibile per l'utente successivo). Successivamente, il codice determina se l'utente ha effettuato l'accesso al sito prima di visualizzare le pagine nella cartella. Se l'utente non è connesso, il codice reindirizza alla pagina di accesso. Pagina di accesso può restituire l'utente alla pagina richiesta originariamente se si include un valore di stringa di query denominato `ReturnUrl`.
5. Creare una nuova pagina di *AuthenticatedContent* cartella denominata *Page.cshtml*.
6. Sostituire il markup predefinito con quanto segue:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Eseguire *Page.cshtml* in un browser. Il codice si viene reindirizzati a una pagina di accesso. È necessario registrare prima di accedere. Dopo aver registrato e connesso, è possibile passare alla pagina e visualizzarne il contenuto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Introduzione a ASP.NET Web Pages con sintassi Razor di programmazione](https://go.microsoft.com/fwlink/?LinkID=251587)
