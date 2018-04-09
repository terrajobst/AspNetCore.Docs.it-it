---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Invio di posta elettronica da un Web ASP.NET di pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo capitolo viene illustrato come inviare un messaggio di posta elettronica automatizzati da un sito Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 9be242d238c627a9557fe7ff7e596974e5b7d1c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>L'invio di posta elettronica dal sito Web ASP.NET (Razor) pagine
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come inviare un messaggio di posta elettronica da un sito Web quando si utilizzano pagine Web ASP.NET (Razor).
> 
> Illustra quanto segue:
> 
> - Come inviare un messaggio di posta elettronica dal sito Web.
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
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>L'invio di messaggi di posta elettronica dal sito Web

Sono disponibili tutti i tipi di motivi per cui si potrebbe essere necessario per inviare posta elettronica dal sito Web. È possibile inviare messaggi di conferma per gli utenti oppure è possibile inviare notifiche a se stessi (ad esempio, che ha registrato un nuovo utente.) Il `WebMail` helper rende più semplice per inviare posta elettronica.

Utilizzare il `WebMail` supporto, è necessario avere accesso a un server SMTP. (SMTP è l'acronimo di *Simple Mail Transfer Protocol*.) Un server SMTP è un server di posta elettronica che inoltra solo i messaggi al server del destinatario &#8212; è il lato in uscita del messaggio di posta elettronica. Se si utilizza un provider di hosting per il sito Web, probabilmente procedere alla configurazione con messaggio di posta elettronica e in modo da capire che cos'è il nome del server SMTP. Se si lavora all'interno di una rete aziendale, un amministratore o il reparto IT può in genere fornire le informazioni relative a un server SMTP da utilizzare. Se si lavora da casa, potrebbe anche essere in grado di eseguire una prova utilizzando il provider di posta elettronica normale, che è possibile indicare il nome del server SMTP. In genere necessario:

- Il nome del server SMTP.
- Il numero di porta. Questo è quasi sempre 25. Tuttavia, il provider potrebbe essere necessario l'utilizzo della porta 587. Se si utilizza protocollo sockets layer (SSL) per la posta elettronica, potrebbe essere una porta diversa. Verificare con il provider di posta elettronica.
- Credenziali (nome utente, password).

In questa procedura creare due pagine. La prima pagina dispone di un form che consente agli utenti di immettere una descrizione, come se sono stati riempimento di un modulo di supporto tecnico. La prima pagina invia le informazioni a un'altra pagina. Nella seconda pagina, codice estrae le informazioni dell'utente e invia un messaggio di posta elettronica. Visualizza inoltre un messaggio di conferma che è stata ricevuta la segnalazione del problema.

![[immagine]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Per semplificare questo esempio, il codice inizializza il `WebMail` destra helper nella pagina in cui viene utilizzato. Tuttavia, per i siti Web reale, è preferibile inserire codice di inizializzazione simile al seguente in un file globale, in modo che la si inizializza il `WebMail` helper per tutti i file nel sito Web. Per ulteriori informazioni, vedere [personalizzazione di un comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Creare un nuovo sito Web.
2. Aggiungere una nuova pagina denominata *EmailRequest.cshtml* e aggiungere il markup seguente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Si noti che il `action` attributo dell'elemento form è stato impostato su *ProcessRequest.cshtml*. Ciò significa che il form verrà inviato alla pagina anziché nuovamente alla pagina corrente.
3. Aggiungere una nuova pagina denominata *ProcessRequest.cshtml* al sito Web e aggiungere il codice e markup seguenti:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Nel codice, ottenere i valori dei campi che sono stati inviati alla pagina di modulo. Chiamare quindi il `WebMail` dell'helper `Send` per creare e inviare il messaggio di posta elettronica. In questo caso, i valori da utilizzare sono costituiti da testo che si concatena con i valori che sono stati inviati dal modulo.

    Il codice per questa pagina è all'interno di un `try/catch` blocco. Se per qualsiasi motivo, il tentativo di inviare un messaggio di posta elettronica non funziona (ad esempio, le impostazioni non sono corrette), il codice di `catch` blocco viene eseguito e imposta il `errorMessage` variabile all'errore che si è verificato. (Per ulteriori informazioni su `try/catch` blocchi o `<text>` tag, vedere [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Nel corpo della pagina, se il `errorMessage` variabile è vuota (impostazione predefinita), l'utente visualizza un messaggio che è stato inviato il messaggio di posta elettronica. Se il `errorMessage` variabile è impostata su true, l'utente vede un messaggio che si è verificato un problema durante l'invio del messaggio.

    Si noti che nella parte della pagina che visualizza un messaggio di errore, è un test: `if(debuggingFlag)`. Si tratta di una variabile che è possibile impostare su true se si verificano problemi durante l'invio di posta elettronica. Quando `debuggingFlag` è true, e se è presente un problema durante l'invio di posta elettronica, viene visualizzato un messaggio di errore aggiuntivi che mostra tutti i valori ASP.NET ha rilevato durante il tentativo di inviare il messaggio di posta elettronica. Avviso equa, tuttavia: i messaggi di errore ASP.NET segnala quando Impossibile inviare un messaggio di posta elettronica possono essere generici. Ad esempio, se ASP.NET non riesce a contattare il server SMTP (ad esempio, perché commesso un errore nel nome del server), l'errore è `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importante** quando si ottiene un messaggio di errore da un oggetto eccezione (`ex` nel codice), eseguire *non* regolarmente passare tale messaggio tramite agli utenti. Oggetti eccezione spesso includono informazioni che gli utenti non devono essere visualizzati e che può essere anche una vulnerabilità di sicurezza. Ecco perché questo codice include la variabile `debuggingFlag` che viene utilizzata come opzione per visualizzare il messaggio di errore e perché la variabile per impostazione predefinita è impostata su false. È necessario impostare la variabile su true (e pertanto visualizzare il messaggio di errore) *solo* se si riscontra un problema con l'invio di posta elettronica ed è necessario eseguire il debug. Dopo avere risolto eventuali problemi, impostare `debuggingFlag` reimpostata su false.

    Modificare le impostazioni correlate nel codice di posta elettronica seguenti:

   - Impostare `your-SMTP-host` sul nome del server SMTP che è possibile accedere.
   - Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.
   - Impostare `your-account-password` la password per l'account del server SMTP.
   - Impostare `your-email-address-here` sul proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio. (Alcuni provider di posta elettronica non consentono di specificare un altro `From` indirizzi e utilizzerà il nome utente come il `From` indirizzo.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Configurazione delle impostazioni di posta elettronica
     > 
     > Può essere un problema in alcuni casi, per assicurarsi di avere le impostazioni corrette per il server SMTP, numero di porta e così via. Di seguito sono riportati alcuni suggerimenti:
     > 
     > - Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`. Tuttavia, se si pubblica il sito per un provider di hosting, il nome del server SMTP a questo punto potrebbe essere `localhost`. Questo avviene perché dopo avere pubblicato e il sito è in esecuzione nel server del provider, il server di posta elettronica potrebbe essere locale dal punto di vista dell'applicazione. Questa modifica nei nomi di server potrebbe indicare che si desidera modificare il nome del server SMTP come parte del processo di pubblicazione.
     > - In genere, il numero di porta è 25. Tuttavia, alcuni provider richiedono l'utilizzo della porta 587 o alcune altre porte.
     > - Assicurarsi di utilizzare le credenziali corrette. Se il sito è stato pubblicato in un provider di hosting, utilizzare le credenziali che il provider ha specificamente indicato per la posta elettronica. Questi potrebbero essere diversi dalle credenziali che utilizzare per la pubblicazione.
     > - In alcuni casi non è necessario credenziali affatto. Se si invia posta elettronica usando l'account personale, provider di posta elettronica potrebbe essere già conoscere le credenziali. Dopo la pubblicazione, potrebbe essere necessario utilizzare credenziali diverse rispetto a quando si testa il computer locale.
     > - Se il provider di posta elettronica utilizza la crittografia, è necessario impostare `WebMail.EnableSsl` a `true`.
4. Eseguire il *EmailRequest.cshtml* pagina in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.)
5. Immettere il nome e una descrizione del problema e quindi scegliere il **Invia** pulsante. Si viene reindirizzati al *ProcessRequest.cshtml* pagina, che conferma il messaggio e che invia un messaggio di posta elettronica. 

    ![[immagine]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Invio di un File tramite posta elettronica

È anche possibile inviare i file allegati per messaggi di posta elettronica. In questa procedura creare un file di testo e due pagine HTML. Si userà il file di testo come allegato di posta elettronica.

1. Nel sito Web, aggiungere un nuovo file di testo e denominarlo *MyFile.txt*.
2. Copiare il testo seguente e incollarlo nel file: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Creare una pagina denominata *SendFile.cshtml* e aggiungere il markup seguente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Creare una pagina denominata *ProcessFile.cshtml* e aggiungere il markup seguente: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modificare le impostazioni correlate nel codice dell'esempio di posta elettronica seguenti:

    - Impostare `your-SMTP-host` per il nome di un server SMTP che è possibile accedere.
    - Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.
    - Impostare `your-email-address-here` sul proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio.
    - Impostare `your-account-password` la password per l'account del server SMTP.
    - Impostare `target-email-address-here` sul proprio indirizzo di posta elettronica. (Come prima, in genere inviando un messaggio di posta elettronica a un altro utente, ma per il test, è possibile inviarlo a se stessi).
6. Eseguire il *SendFile.cshtml* pagina in un browser.
7. Immettere il nome, la riga dell'oggetto e il nome del file di testo per collegare (*MyFile.txt*).
8. Fare clic sul pulsante `Submit`. Come prima, si viene reindirizzati al *ProcessFile.cshtml* pagina, che conferma il messaggio e che invia un messaggio di posta elettronica con il file allegato.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
