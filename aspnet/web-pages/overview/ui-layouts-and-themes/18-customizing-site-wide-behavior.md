---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizzazione del comportamento a livello di sito per ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: In questo capitolo illustra come definire impostazioni per l'intero sito Web o un'intera cartella anziché semplicemente una pagina.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 3930c1cceb86e4105463323e0896d7491af5ae56
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391932"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalizzazione del comportamento a livello di sito per i siti di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come effettuare impostazioni sito-side per le pagine in un sito Web di ASP.NET Web Pages (Razor).
> 
> Che cosa si apprenderà come:
> 
> - Come eseguire il codice che consente di set di valori (i valori globali o le impostazioni di supporto) per tutte le pagine in un sito.
> - Come eseguire il codice che consente di impostare i valori per tutte le pagine in una cartella.
> - Come eseguire codice prima e dopo una pagina viene caricata.
> - Come inviare errori a una pagina di errore centrale.
> - Come aggiungere l'autenticazione a tutte le pagine in una cartella.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (pacchetto NuGet)
>   
> 
> Questa esercitazione funziona anche con 3 pagine Web ASP.NET e Visual Studio 2013 o Visual Studio Express 2013 per Web, è possibile usare ASP.NET Web Helpers Library.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Aggiunta di codice di avvio del sito Web per ASP.NET Web Pages

Per la maggior parte del codice scritto in ASP.NET Web Pages, una singola pagina possa contenere tutto il codice necessario per la pagina. Ad esempio, se una pagina invia un messaggio di posta elettronica, è possibile inserire tutto il codice per l'operazione in una singola pagina. Ciò può includere il codice per inizializzare le impostazioni per l'invio di posta elettronica (vale a dire, per il server SMTP) e per l'invio del messaggio di posta elettronica.

Tuttavia, in alcune situazioni, potrebbe voler eseguire il codice prima dell'esecuzione di qualsiasi pagina nel sito. Ciò è utile per l'impostazione di valori che possono essere utilizzati ovunque nel sito (detta *i valori globali*.) Ad esempio, alcuni helper richiedono di fornire i valori come impostazioni di posta elettronica o le chiavi dell'account. Può essere utile per mantenere queste impostazioni nei valori globali.

È possibile farlo creando una pagina denominata  *\_AppStart.cshtml* nella radice del sito. Se questa pagina è presente, viene eseguita la prima volta che viene richiesta qualsiasi pagina nel sito. Pertanto, è un buon punto di esecuzione di codice per impostare i valori globali. (Poiché  *\_AppStart.cshtml* ha un prefisso di un carattere di sottolineatura, non invierà la pagina ASP.NET in un browser anche se gli utenti richiedono direttamente.)

Il diagramma seguente mostra come la  *\_AppStart.cshtml* pagina funziona. Quando viene ricevuta una richiesta per una pagina e se si tratta della prima richiesta di qualsiasi pagina nel sito ASP.NET controlla prima se un  *\_AppStart.cshtml* esistente. In questo caso, qualsiasi codice nel  *\_AppStart.cshtml* esecuzioni di pagina e quindi esegue la pagina richiesta.

![[immagine]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Impostazione dei valori globali per il sito Web

1. Nella cartella radice di un sito Web di WebMatrix, creare un file denominato  *\_AppStart.cshtml*. Il file deve essere nella radice del sito.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Questo codice archivia un valore nel `AppState` dizionario, che è disponibile automaticamente in tutte le pagine nel sito. Si noti che il  *\_AppStart.cshtml* file non dispone di alcun markup in esso. La pagina verrà eseguito il codice e quindi viene reindirizzato alla pagina richiesto originariamente.

    > [!NOTE]
    > Prestare attenzione quando si inserisce codice  *\_AppStart.cshtml* file. Se si verificano errori nel codice nel  *\_AppStart.cshtml* file, non si avvia il sito Web.
3. Nella cartella radice, creare una nuova pagina denominata *AppName.cshtml*.
4. Sostituire il markup predefinito e il codice con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Questo codice estrae il valore dal `AppState` oggetto che viene impostato nel  *\_AppStart.cshtml* pagina.
5. Eseguire la *AppName.cshtml* pagina in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) La pagina Visualizza il valore globale. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Valori delle impostazioni per gli helper

Un utilizzo corretto per il  *\_AppStart.cshtml* file consiste nell'impostare i valori per gli helper che utilizzano nel proprio sito e che devono essere inizializzati. Esempi tipici sono le impostazioni di posta elettronica per il `WebMail` helper e le chiavi pubbliche e private per il `ReCaptcha` helper. In questi casi, è possibile impostare i valori di una volta il  *\_AppStart.cshtml* e quindi si ha già impostati per tutte le pagine nel sito.

Questa procedura viene illustrato come impostare `WebMail` impostazioni a livello globale. (Per altre informazioni sull'uso di `WebMail` helper, vedere [aggiunta di posta elettronica a un sito con pagine Web ASP.NET](../getting-started/11-adding-email-to-your-web-site.md).)

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.
2. Se si ha già un  *\_AppStart.cshtml* file, nella cartella radice di un sito Web, creare un file denominato  *\_AppStart.cshtml*.
3. Aggiungere il codice seguente `WebMail` le impostazioni per il  *\_AppStart.cshtml* file: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modificare le impostazioni correlate nel codice di posta elettronica seguenti:

   - Impostare `your-SMTP-host` sul nome del server SMTP che è possibile utilizzare.
   - Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.
   - Impostare `your-account-password` alla password per l'account del server SMTP.
   - Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica. Questo è il messaggio viene inviato dall'indirizzo di posta elettronica. (Alcuni provider di posta elettronica non consentono di specificare un diverso `From` indirizzi e userà il nome utente come il `From` indirizzo.)

     Per altre informazioni sulle impostazioni SMTP, vedere [configurazione delle impostazioni di posta elettronica](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) nell'articolo [l'invio di posta elettronica da un sito di ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) e [problemi con l'invio di posta elettronica](https://go.microsoft.com/fwlink/?LinkId=253001#email)nel [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Salvare il  *\_AppStart.cshtml* file e chiuderlo.
5. Nella cartella radice di un sito Web, creare nuova pagina denominata *TestEmail.cshtml*.
6. Sostituire il contenuto esistente con il codice seguente: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Eseguire la *TestEmail.cshtml* pagina in un browser.
8. Compilare i campi da inviare a se stessi un messaggio di posta elettronica e quindi fare clic su **inviare**.
9. Controllare la posta elettronica per assicurarsi che è stato ricevuto il messaggio.

La parte importante di questo esempio è che le impostazioni che in genere non cambiano, come il nome del server SMTP e le credenziali del messaggio di posta elettronica, ovvero vengono impostati  *\_AppStart.cshtml* file. In questo modo che non è necessario impostarle di nuovo in ogni pagina in cui si invia tramite posta elettronica. (Sebbene se per qualche motivo è necessario modificare queste impostazioni, è possibile impostarle singolarmente in una pagina.) Nella pagina, è solo necessario impostare i valori che in genere modificato ogni volta, come il destinatario e il corpo del messaggio di posta elettronica.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Esecuzione del codice prima e dopo i file in una cartella

Come è possibile usare  *\_AppStart.cshtml* per scrivere il codice prima di eseguire le pagine del sito, è possibile scrivere codice che viene eseguito prima (e successive) in una particolare cartella eseguire qualsiasi pagina. Ciò è utile per operazioni come l'impostazione layout coinvolte per tutte le pagine in una cartella o per la verifica che un utente è connesso prima di eseguire una pagina nella cartella.

Per le pagine in particolare le cartelle, è possibile creare codice in un file denominato  *\_Pagestart*. Il diagramma seguente mostra come la  *\_Pagestart* pagina funziona. Quando viene ricevuta una richiesta per una pagina, ASP.NET esegue la ricerca di un  *\_AppStart.cshtml* pagina e che viene eseguito. Quindi ASP.NET controlla se vi sia un  *\_Pagestart* pagina e in questo caso, viene eseguito. Viene quindi eseguita la pagina richiesta.

All'interno di  *\_Pagestart* pagina, è possibile specificare la posizione durante l'elaborazione si desidera che la pagina richiesta per l'esecuzione includendo un `RunPage` (metodo). Ciò consente di eseguire codice prima esecuzione della pagina richiesta e quindi nuovamente dopo di esso. Se non si include `RunPage`, tutto il codice incluso  *\_Pagestart* esecuzioni, quindi la pagina richiesta viene eseguita automaticamente.

![[immagine]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET consente di creare una gerarchia di  *\_Pagestart* file. È possibile inserire un  *\_Pagestart* file nella radice del sito e in qualsiasi sottocartella. Quando viene richiesta una pagina, il  *\_Pagestart* file di esecuzione del livello più alto (più vicina alla radice del sito), seguita dal  *\_Pagestart* file entro i prossimi sottocartella, e così via verso il basso la struttura di sottocartelle fino a quando la richiesta raggiunga la cartella che contiene la pagina richiesta. Dopo tutte le applicabili  *\_Pagestart* esegue i file, viene eseguita la pagina richiesta.

Ad esempio, potrebbe essere la seguente combinazione di  *\_Pagestart* i file e *default. cshtml* file:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Quando si esegue */myfolder/default.cshtml*, si noterà quanto segue:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Esecuzione del codice di inizializzazione per tutte le pagine in una cartella

Un utilizzo corretto per  *\_Pagestart* file consiste nell'inizializzare la pagina di layout stesso per tutti i file in un'unica cartella.

1. Nella cartella radice, creare una nuova cartella denominata *InitPages*.
2. Nel *InitPages* cartella del sito Web, creare un file denominato  *\_Pagestart* e sostituire il markup predefinito e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Nella radice del sito Web, creare una cartella denominata *condiviso*.
4. Nel *Shared* cartella, creare un file denominato  *\_Layout1.cshtml* e sostituire il markup predefinito e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. Nel *InitPages* cartella, creare un file denominato *Content1.cshtml* e sostituire il contenuto esistente con il codice seguente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. Nel *InitPages* cartella, creare un altro file denominato *Content2.cshtml* e sostituire il markup predefinito con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Eseguire *Content1.cshtml* in un browser. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Quando la *Content1.cshtml* pagina esecuzioni, la  *\_Pagestart* file imposta `Layout` e imposta anche `PageData["MyBackground"]` a un colore. Nelle *Content1.cshtml*, vengono applicati il layout e colore.
8. Display *Content2.cshtml* in un browser. 

    Il layout è lo stesso, perché entrambe le pagine utilizzano la stessa pagina layout e colore come inizializzate  *\_Pagestart*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Usando \_Pagestart per gestire gli errori

Un'altra buona Usa per la  *\_Pagestart* file consiste nel creare un modo per gestire gli errori di programmazione (eccezioni) che potrebbero verificarsi in qualsiasi *cshtml* pagina in una cartella. In questo esempio illustra un modo per eseguire questa operazione.

1. Nella cartella radice, creare una cartella denominata *InitCatch*.
2. Nel *InitCatch* cartella del sito Web, creare un file denominato  *\_Pagestart* e sostituire il markup esistente e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    In questo codice è provare a eseguire la pagina richiesta in modo esplicito chiamando il `RunPage` metodo all'interno di un `try` blocco. Se si verificano eventuali errori di programmazione nella richiesta di pagina, il codice all'interno di `catch` bloccare l'esecuzione. In questo caso, il codice reindirizza a una pagina (*Error.cshtml*) e passa il nome del file che si è verificato l'errore come parte dell'URL. (Verrà creata la pagina al più presto.)
3. Nel *InitCatch* cartella del sito Web, creare un file denominato *Exception.cshtml* e sostituire il markup esistente e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Ai fini di questo esempio, ciò che si sta eseguendo in questa pagina è deliberatamente la creazione di un errore quando si tenta di aprire un file di database che non esiste.
4. Nella cartella radice, creare un file denominato *Error.cshtml* e sostituire il markup esistente e il codice con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    In questa pagina, l'espressione `@Request["source"]` Ottiene il valore non compreso nell'URL e lo visualizza.
5. Nella barra degli strumenti, fare clic su **salvare**.
6. Eseguire *Exception.cshtml* in un browser. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Perché si verifica un errore *Exception.cshtml*, il  *\_Pagestart* pagina reindirizza al *Error.cshtml* file, che viene visualizzato il messaggio.

    Per altre informazioni sulle eccezioni, vedere [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Usando \_Pagestart per limitare l'accesso alle cartelle

È anche possibile usare la  *\_Pagestart* file per limitare l'accesso a tutti i file in una cartella.

1. In WebMatrix, creare un nuovo sito Web usando il **sito da modello** opzione.
2. Tra i modelli disponibili, selezionare **Starter Site**.
3. Nella cartella radice, creare una cartella denominata *AuthenticatedContent*.
4. Nel *AuthenticatedContent* cartella, creare un file denominato  *\_Pagestart* e sostituire il markup esistente e il codice con quanto segue: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Il codice avvia impedendo la memorizzazione nella cache di tutti i file nella cartella. (Ciò è necessario per scenari come i computer pubblici, in cui non si desidera pagine memorizzate nella cache di un utente specifico sia disponibile per l'utente successivo). Successivamente, il codice determina se l'utente ha effettuato l'accesso al sito prima di visualizzare le pagine nella cartella. Se l'utente non è connesso, il codice reindirizza alla pagina di accesso. La pagina di accesso può restituire l'utente alla pagina originariamente richiesta se si include un valore di stringa di query denominato `ReturnUrl`.
5. Creare una nuova pagina nel *AuthenticatedContent* cartella denominata *Page.cshtml*.
6. Sostituire il markup predefinito con il codice seguente:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Eseguire *Page.cshtml* in un browser. Il codice si viene reindirizzati a una pagina di accesso. È necessario registrarsi prima di accedere. Dopo aver registrato e connesso, è possibile passare alla pagina e visualizzarne il contenuto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
