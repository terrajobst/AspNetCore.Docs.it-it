---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: L'invio di posta elettronica da un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: In questo capitolo illustra come inviare un messaggio di posta elettronica automatizzati da un sito Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 4429b9df0a6bc689eb3549c13aee598353c438af
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823802"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>L'invio di posta elettronica da un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come inviare un messaggio di posta elettronica da un sito Web quando si usa ASP.NET Web Pages (Razor).
> 
> Che cosa si apprenderà come:
> 
> - Come inviare un messaggio di posta elettronica tramite il sito Web.
> - Come collegare un file a un messaggio di posta elettronica.
> 
> Si tratta della funzionalità ASP.NET introdotta nell'articolo:
> 
> - Il `WebMail` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>L'invio di messaggi di posta elettronica tramite il sito Web

Esistono molti motivi per cui potrebbe essere necessario inviare posta elettronica tramite il sito Web. È possibile inviare i messaggi di conferma per gli utenti oppure è possibile inviare notifiche a se stessi (ad esempio, che ha registrato un nuovo utente). Il `WebMail` helper rende più semplice per inviare posta elettronica.

Usare il `WebMail` helper, è necessario avere accesso a un server SMTP. (SMTP è l'acronimo *Simple Mail Transfer Protocol*.) Un server SMTP è un server di posta elettronica che inoltra solo messaggi al server del destinatario &#8212; è il lato in uscita del messaggio di posta elettronica. Se si usa un provider di hosting per il sito Web, probabilmente procedere alla configurazione con la posta elettronica e in modo da capire che cos'è il nome del server SMTP. Se si lavora all'interno di una rete aziendale, un amministratore o il reparto IT può in genere fornire le informazioni su un server SMTP che è possibile usare. Se si lavora da casa, è anche possibile eseguire la verifica utilizzando il provider di posta elettronica normale, che è possibile indicare il nome del proprio server SMTP. È in genere necessario:

- Il nome del server SMTP.
- Il numero di porta. Si tratta quasi sempre di 25. Tuttavia, potrebbe essere necessario usare la porta 587 ISP. Se si usa (SSL) secure sockets layer per la posta elettronica, si potrebbe essere una porta diversa. Rivolgersi al provider di posta elettronica.
- Credenziali (nome utente, password).

In questa procedura, creare due pagine. La prima pagina ha un formato che consente agli utenti di immettere una descrizione, come se essi sono stati compilando un modulo di supporto tecnico. La prima pagina invia le informazioni a un'altra pagina. Nella seconda pagina, codice estrae le informazioni dell'utente e invia un messaggio di posta elettronica. Visualizza inoltre un messaggio di conferma che è stata ricevuta la segnalazione del problema.

![[immagine]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Per semplificare questo esempio, il codice inizializza il `WebMail` destra helper della pagina in cui viene utilizzato. Tuttavia, per i siti Web reali, è preferibile inserire codice di inizializzazione simile al seguente in un file globale, in modo che si inizializza il `WebMail` helper per tutti i file nel sito Web. Per altre informazioni, vedere [personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Creare un nuovo sito Web.
2. Aggiungere una nuova pagina denominata *EmailRequest.cshtml* e aggiungere il markup seguente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Si noti che il `action` attributo dell'elemento del form è stato impostato su *ProcessRequest.cshtml*. Ciò significa che il modulo verrà inviato a tale pagina anziché indietro alla pagina corrente.
3. Aggiungere una nuova pagina denominata *ProcessRequest.cshtml* al sito Web e aggiungere il codice e il markup seguente:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Nel codice, è ottenere i valori dei campi modulo che sono stati inviati alla pagina. È quindi possibile chiamare il `WebMail` dell'helper `Send` metodo per creare e inviare il messaggio di posta elettronica. In questo caso, i valori da utilizzare sono costituiti da testo che si concatena con i valori che sono stati inviati dal modulo.

    Il codice per questa pagina è all'interno di un `try/catch` blocco. Se per qualsiasi motivo il tentativo di inviare un messaggio di posta elettronica non funziona correttamente (ad esempio, le impostazioni non sono a destra), il codice nel `catch` blocco viene eseguito e imposta il `errorMessage` variabile all'errore che si è verificato. (Per altre informazioni sulle `try/catch` blocchi o le `<text>` tag, vedere [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Nel corpo della pagina, se il `errorMessage` variabile è vuota (impostazione predefinita), l'utente visualizza un messaggio che è stato inviato il messaggio di posta elettronica. Se il `errorMessage` variabile è impostata su true, l'utente vede un messaggio che si è verificato un problema durante l'invio del messaggio.

    Si noti che nella parte della pagina che visualizza un messaggio di errore, non esiste un test aggiuntivi: `if(debuggingFlag)`. Questa è una variabile che è possibile impostare su true se si verificano problemi durante l'invio di posta elettronica. Quando si `debuggingFlag` è true, e se è presente un problema durante l'invio di posta elettronica, viene visualizzato un messaggio di errore aggiuntive che mostra tutti i valori ASP.NET ha segnalato durante il tentativo di inviare il messaggio di posta elettronica. Avviso corretto, però: i messaggi di errore che segnala di ASP.NET quando Impossibile inviare un messaggio di posta elettronica possono essere generici. Ad esempio, se ASP.NET non riesce a contattare il server SMTP (ad esempio, perché è stato commesso un errore nel nome del server), l'errore è `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importanti** quando viene visualizzato un messaggio di errore da un oggetto eccezione (`ex` nel codice), si *non* regolarmente passare tale messaggio tramite agli utenti. Oggetti eccezione includono spesso informazioni che gli utenti non devono essere visualizzati e che può essere anche una vulnerabilità di sicurezza. Ecco perché questo codice include la variabile `debuggingFlag` che viene usato come un'opzione per visualizzare il messaggio di errore e il motivo per cui la variabile per impostazione predefinita è impostata su false. È consigliabile impostare tale variabile su true (e pertanto visualizzare il messaggio di errore) *solo* se si riscontra un problema con l'invio di posta elettronica ed è necessario eseguire il debug. Dopo aver risolto eventuali problemi, impostare `debuggingFlag` reimpostarla su false.

    Modificare le impostazioni correlate nel codice di posta elettronica seguenti:

   - Impostare `your-SMTP-host` sul nome del server SMTP che è possibile utilizzare.
   - Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.
   - Impostare `your-account-password` alla password per l'account del server SMTP.
   - Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica. Questo è il messaggio viene inviato dall'indirizzo di posta elettronica. (Alcuni provider di posta elettronica non consentono di specificare un diverso `From` indirizzi e userà il nome utente come il `From` indirizzo.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Configurazione delle impostazioni di posta elettronica
     > 
     > Può essere un problema in alcuni casi per assicurarsi di avere le impostazioni corrette per il server SMTP, numero di porta e così via. Di seguito sono riportati alcuni suggerimenti:
     > 
     > - Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`. Tuttavia, se si pubblica il sito per un provider di hosting, il nome del server SMTP a quel punto possibile `localhost`. Questo avviene perché dopo aver pubblicato e il sito è in esecuzione nel server del provider, il server di posta elettronica potrebbe essere locale dal punto di vista dell'applicazione. Questa modifica nei nomi di server potrebbe essere che necessario modificare il nome del server SMTP come parte del processo di pubblicazione.
     > - Il numero di porta in genere è 25. Tuttavia, alcuni provider richiedono di usare la porta 587 o alcune porte.
     > - Assicurarsi di usare le credenziali corrette. Se il sito è stata pubblicata in un provider di hosting, usare le credenziali che il provider ha segnalato in modo specifico sono per la posta elettronica. Questi potrebbero essere diversi dalle credenziali che utilizzare per la pubblicazione.
     > - In alcuni casi non occorre credenziali affatto. Se si sta inviando e-mail tramite all'account personale, il provider di posta elettronica sappia già le proprie credenziali. Dopo aver pubblicato, è necessario usare credenziali diverse da quelle quando si testa nel computer locale.
     > - Se il provider di posta elettronica utilizza la crittografia, è necessario impostare `WebMail.EnableSsl` a `true`.
4. Eseguire la *EmailRequest.cshtml* pagina in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.)
5. Immettere il nome e una descrizione del problema e quindi scegliere il **Submit** pulsante. Si verrà reindirizzati per il *ProcessRequest.cshtml* pagina per confermare che il messaggio e che invia un messaggio di posta elettronica. 

    ![[immagine]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>L'invio di un File tramite posta elettronica

È anche possibile inviare i file collegati per i messaggi di posta elettronica. In questa procedura, si crea un file di testo e due pagine HTML. Si userà il file di testo come allegato di posta elettronica.

1. Nel sito Web, aggiungere un nuovo file di testo e denominarlo *MyFile*.
2. Copiare il testo seguente e incollarlo nel file: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Creare una pagina denominata *SendFile.cshtml* e aggiungere il markup seguente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Creare una pagina denominata *ProcessFile.cshtml* e aggiungere il markup seguente: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modificare le impostazioni correlate nel codice dell'esempio di posta elettronica seguenti:

    - Impostare `your-SMTP-host` al nome di un server SMTM che è possibile utilizzare.
    - Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.
    - Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica. Questo è il messaggio viene inviato dall'indirizzo di posta elettronica.
    - Impostare `your-account-password` alla password per l'account del server SMTP.
    - Impostare `target-email-address-here` per il proprio indirizzo di posta elettronica. (Come prima, si sarebbe in genere invia un messaggio di posta elettronica a un altro utente, ma per i test, è possibile inviarlo a se stessi).
6. Eseguire la *SendFile.cshtml* pagina in un browser.
7. Immettere il nome del file di testo per collegare il proprio nome e una riga dell'oggetto (*MyFile*).
8. Fare clic sul pulsante `Submit`. Come in precedenza, si verrà reindirizzati per il *ProcessFile.cshtml* pagina per confermare che il messaggio e che invia un messaggio di posta elettronica con il file allegato.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
