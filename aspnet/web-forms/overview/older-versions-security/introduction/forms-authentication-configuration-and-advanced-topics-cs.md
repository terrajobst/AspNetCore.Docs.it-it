---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: I moduli di configurazione dell'autenticazione e argomenti avanzati (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verranno esaminare le varie impostazioni di autenticazione form e informazioni su come modificarle tramite l'elemento di form. Ciò comporterà un dettagliate...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: 707c47917450bad409a04b3b5b80edc560b7c756
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372774"
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Configurazione dell'autenticazione form e argomenti avanzati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> In questa esercitazione verranno esaminare le varie impostazioni di autenticazione form e informazioni su come modificarle tramite l'elemento di form. Ciò comporterà un quadro dettagliato di personalizzazione di valore di timeout del ticket di autenticazione form, usando una pagina di accesso con un URL personalizzato (ad esempio SignIn.aspx anziché Login. aspx) e i ticket di autenticazione form senza cookie.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](an-overview-of-forms-authentication-cs.md) abbiamo esaminato i passaggi necessari per l'implementazione di autenticazione basata su form in un'applicazione ASP.NET, dalla specifica delle impostazioni di configurazione in Web. config per la creazione di un log nella pagina per visualizzare diversi contenuto per gli utenti autenticati e anonimi. È importante ricordare che è stato configurato il sito Web per usare l'autenticazione basata su form, impostando l'attributo mode del &lt;autenticazione&gt; elemento a un form. Il &lt;authentication&gt; elemento può includere facoltativamente un &lt;forms&gt; elemento figlio, attraverso il quale si può specificare un'ampia gamma di impostazioni di autenticazione form.

In questa esercitazione verranno esaminare le varie impostazioni di autenticazione form e verrà illustrato come modificarli tramite il &lt;form&gt; elemento. Ciò comporterà un quadro dettagliato di personalizzazione di valore di timeout del ticket di autenticazione form, usando una pagina di accesso con un URL personalizzato (ad esempio SignIn.aspx anziché Login. aspx) e i ticket di autenticazione form senza cookie. Verranno inoltre esaminare più da vicino la composizione del ticket di autenticazione form e verrà illustrato le precauzioni che ASP.NET necessario per garantire la sicurezza da ispezione e la manomissione dei dati del ticket. Infine, si esaminerà come archiviare i dati utente aggiuntivi del ticket di autenticazione form e su come modellare i dati tramite un oggetto entità personalizzato.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Passaggio 1: Esaminare i &lt;form&gt; le impostazioni di configurazione

Il sistema di autenticazione form in ASP.NET offre una serie di impostazioni di configurazione che possono essere personalizzati in modo da applicazione ad. Ciò include le impostazioni, ad esempio: la durata dell'autenticazione form del ticket; il tipo di protezione viene applicato al ticket; con l'autenticazione senza cookie quali condizioni vengono utilizzati i ticket; il percorso alla pagina di accesso; e altre informazioni. Per modificare i valori predefiniti, aggiungere un [ &lt;form&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) come elemento figlio di [ &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx), specificando le proprietà i valori desiderati per personalizzare come attributi XML nel modo seguente:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Tabella 1 sono riepilogate le proprietà che possono essere personalizzate tramite il &lt;form&gt; elemento. Poiché Web. config è un file XML, i nomi di attributo nella colonna sinistra sono tra maiuscole e minuscole.


| <strong>Attributo</strong> |                                                                                                                                                                                                                                     <strong>Descrizione</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         senza cookie         |                                                                                                                Questo attributo specifica in quali condizioni il ticket di autenticazione verrà archiviato in un cookie e viene incorporato nell'URL. I valori consentiti sono: UseCookies; UseUri; Rilevamento automatico; e UseDeviceProfile (predefinito). Passaggio 2 viene esaminato questa impostazione in modo più dettagliato.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica l'URL che gli utenti vengono reindirizzati a dopo l'accesso dalla pagina di accesso se è presente alcun valore di URL di reindirizzamento specificato nella stringa di query. Il valore predefinito è default. aspx.                                                                                                                                                         |
|           dominio           | Quando si usano i ticket di autenticazione basata su cookie, questa impostazione specifica il valore di dominio del cookie. Il valore predefinito è una stringa vuota, che consente al browser di usare il dominio da cui è stato rilasciato (ad esempio www.yourdomain.com). In questo caso, il cookie verrà <strong>non</strong> da inviare quando sottodomini, ad esempio admin.yourdomain.com richiede la creazione. Se si desidera che il cookie deve essere passato a tutti i sottodomini che è necessario personalizzare l'attributo del dominio impostandolo su dominio.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Valore booleano che indica se gli utenti autenticati vengano memorizzati quando reindirizzati a URL in altre applicazioni web nello stesso server. Il valore predefinito è false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      L'URL della pagina di accesso. Il valore predefinito è Login. aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Quando si usano i ticket di autenticazione basata su cookie, il nome del cookie. Il valore predefinito è. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Quando si usano i ticket di autenticazione basata su cookie, questa impostazione specifica l'attributo per il percorso del cookie. L'attributo path consente agli sviluppatori di limitare l'ambito di un cookie da una gerarchia di directory specifico. Il valore predefinito è /, che informa il browser per inviare il cookie del ticket di autenticazione a qualsiasi richiesta effettuata al dominio.                                                                              |
|         protezione         |                                                                                                                                            Indica quali tecniche usate per proteggere il ticket di autenticazione form. I valori consentiti sono: tutti (impostazione predefinita); Crittografia; None; e la convalida. Queste impostazioni sono illustrate in dettaglio nel passaggio 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Valore booleano che indica se trasmettere il cookie di autenticazione è necessaria una connessione SSL. Il valore predefinito è false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Valore booleano che indica che se il timeout del cookie di autenticazione viene reimpostato ogni volta che l'utente visita il sito durante una singola sessione. Il valore predefinito è true. I criteri di timeout ticket di autenticazione vengano discusso in maggior dettaglio nella specifica sezione relativa al valore di Timeout del Ticket.                                                                                                 |
|          timeout           |                                                                                                                               Specifica l'ora, minuti, dopo il quale il cookie del ticket di autenticazione prima della scadenza. Il valore predefinito è 30. I criteri di timeout ticket di autenticazione vengano discusso in maggior dettaglio nella specifica sezione relativa al valore di Timeout del Ticket.                                                                                                                               |

**Tabella 1**: un riepilogo delle &lt;form&gt; degli attributi dell'elemento

In ASP.NET 2.0 e versioni successive, l'impostazione predefinita i valori di autenticazione form sono impostate come hardcoded nella classe FormsAuthenticationConfiguration in .NET Framework. Su base dall'applicazione nel file Web. config, è necessario applicare alcuna modifica. Questo comportamento è diverso da ASP.NET 1.x, i valori di autenticazione form predefinito in cui sono stati archiviati nel file Machine. config (e pertanto può essere modificati tramite la modifica di file Machine. config). Periodo di tempo per l'argomento di ASP.NET 1.x, vale la pena menzionare che un numero di impostazioni di sistema di autenticazione form hanno valori predefiniti diversi in ASP.NET 2.0 e oltre a in ASP.NET 1.x. Se si sta migrando l'applicazione da un ambiente di ASP.NET 1.x, è importante essere a conoscenza di queste differenze. Consultare [il &lt;form&gt; documentazione tecnica elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) per un elenco delle differenze.

> [!NOTE]
> Diverse impostazioni di autenticazione form, ad esempio il timeout, dominio e il percorso, specificano i dettagli per il cookie di ticket di autenticazione form risultante. Per altre informazioni sui cookie, sul relativo funzionamento e le relative proprietà diverse, leggere [in questa esercitazione i cookie](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Se si specifica il valore di Timeout del Ticket

Il ticket di autenticazione form è un token che rappresenta un'identità. Con i ticket di autenticazione basata su cookie, questo token verrà mantenuto sotto forma di un cookie e inviato al server web per ogni richiesta. Il possesso del token, in sostanza, viene dichiarata, mi *username*, già effettuato l'accesso e viene usato in modo che un'identità dell'utente possa essere memorizzati nella navigazione tra le visite di pagina.

Il ticket di autenticazione form non solo include l'identità dell'utente, ma contiene anche informazioni utili per assicurare l'integrità e sicurezza del token. Dopo tutto, non vogliamo dannosa all'utente di essere in grado di creare un token contraffatto, o per modificare un token legittimare in qualche modo underhanded.

Un tale bit di informazioni inclusa nel ticket è un' *scadenza*, ovvero la data e ora il ticket non è più valido. Ogni volta che FormsAuthenticationModule esamina un ticket di autenticazione assicura che la scadenza del ticket non è ancora passato. In questo caso, non considera il ticket e identifica l'utente come anonimo. Questa misura aiuta a proteggersi dagli attacchi di riproduzione. Senza una scadenza, se un pirata informatico è riuscito a ottenere propria mani sul ticket di autenticazione valida dell'utente - forse ottenga l'accesso fisico al computer ed rooting attraverso i cookie, è stato possibile inviare una richiesta al server con il ticket di autenticazione e ottenere l'accesso. La scadenza non impedisce in questo scenario, ma limita la finestra durante i quali un attacco può avere esito positivo.

> [!NOTE]
> Passaggio 3 informazioni tecniche aggiuntive utilizzate dal sistema di autenticazione form per proteggere il ticket di autenticazione.


Quando si crea il ticket di autenticazione, il sistema di autenticazione form determina la scadenza del consultando l'impostazione di timeout. Come indicato nella tabella 1, il timeout impostando i valori predefiniti per 30 minuti, il che significa che quando viene creato il ticket di autenticazione form di scadenza è impostata su una data e ora nel futuro di 30 minuti.

La scadenza definisce un'ora assoluta in futuro in cui scade il ticket di autenticazione form. Ma in genere gli sviluppatori vogliono implementare una scadenza variabile, che viene reimpostato ogni volta che l'utente ritorna al sito. Questo comportamento è determinato dalle impostazioni di slidingExpiration. Se impostato su true (impostazione predefinita), ogni volta che un utente viene autenticato FormsAuthenticationModule, aggiorna la scadenza del ticket. Se impostato su false, la scadenza non viene aggiornata a ogni richiesta, determinando il ticket scadere esattamente timeout numero di minuti passati quando il ticket è stato inizialmente creato.

> [!NOTE]
> La scadenza archiviata nel ticket di autenticazione è una data assoluta e il valore di ora, ad esempio il 2 agosto 2008 11 34 AM. Inoltre, la data e ora sono rispetto all'ora locale del server web. Questa decisione progettuale può avere alcuni effetti collaterali interessanti intorno all'ora legale (DST), ovvero quando orologi negli Stati Uniti vengono spostati avanti un'ora (presupponendo che il server web è ospitato nelle impostazioni locali in cui si osserva ora legale). Si consideri cosa accadrebbe per un sito Web ASP.NET con una scadenza di 30 minuti in prossimità l'ora di inizio dell'ora legale (ovvero alle 2.00). Si immagini che un visitatore accede al sito del 11 marzo 2008 alle ore 01:00: 55. Ciò potrebbe generare un ticket di autenticazione form che scade alle 11 marzo 2008 alle 25 2:00 (30 minuti in futuro). Tuttavia, una volta 2:00 versione RTM, l'orologio passa alla 3:00 AM a causa dell'ora legale. Quando l'utente carica una nuova pagina sei minuti dopo l'accesso (alle 01 AM), FormsAuthenticationModule viene rilevato che il ticket è scaduto e l'utente viene reindirizzato alla pagina di accesso. Per una discussione più approfondita su questo e altro comportamento imprevisto dei timeout ticket di autenticazione, nonché le soluzioni alternative, procurarsi una copia di Stefan Schackow *Professional ASP.NET 2.0 Security e l'appartenenza al ruolo Gestione* (ISBN: 978-0-7645-9698-8).


Figura 1 illustra il flusso di lavoro quando slidingExpiration viene impostata su false e timeout è impostato su 30. Si noti che il ticket di autenticazione generato all'account di accesso contiene la data di scadenza, questo valore non viene aggiornato nelle richieste successive. Se la classe FormsAuthenticationModule rileva che il ticket è scaduto, lo ignora e considera la richiesta anonima.


[![Una rappresentazione grafica di slidingExpiration quando la scadenza del Ticket di autenticazione form è false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Figura 01**: una rappresentazione grafica dei slidingExpiration quando la scadenza del Ticket di autenticazione form è false ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


Figura 2 mostra il flusso di lavoro quando slidingExpiration viene impostata su true e il timeout è impostato su 30. Quando viene ricevuta una richiesta autenticata (con un ticket non scaduti) viene aggiornata la scadenza del timeout numero di minuti in futuro.


[![Una rappresentazione grafica del Ticket di autenticazione form quando viene soddisfatta slidingExpiration](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Figura 02**: una rappresentazione grafica del Ticket di autenticazione form quando viene soddisfatta slidingExpiration ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Quando si usano i ticket di autenticazione basata su cookie (predefinito), questa discussione diventa più complicato perché i cookie possono anche avere i propri oggetti scaduti nella specificato. Scadenza del cookie (o l'assenza) indica al browser quando il cookie deve essere eliminato. Se il cookie non ha una scadenza, che è definitivamente quando il browser viene arrestato. Se è presente una scadenza, tuttavia, il cookie rimane memorizzato nel computer dell'utente fino alla data e intervallo di tempo specificato in scadenza. Quando viene eliminato un cookie dal browser, non viene inviato al server web. Pertanto, l'eliminazione di un cookie è analoga all'utente di registrazione all'esterno del sito.

> [!NOTE]
> Naturalmente, un utente può rimuovere in modo proattivo i cookie memorizzati sul proprio computer. In Internet Explorer 7, si potrebbe passare a strumenti, opzioni e fare clic sul pulsante di eliminazione nella sezione della cronologia di esplorazione. Da qui, fare clic sul pulsante Elimina i cookie.


Il sistema di autenticazione form crea basati su sessione o basata su scadenza cookie a seconda del valore passato al *persistCookie* parametro. Tenere presente che i FormsAuthentication metodi della classe GetAuthCookie SetAuthCookie e RedirectFromLoginPage accettano due parametri di input: *nomeutente* e *persistCookie*. La pagina di accesso che è stato creato nell'esercitazione precedente è incluso una memorizza dati controllo CheckBox, determinare se è stato creato un cookie persistente. I cookie permanenti sono basati su scadenza; i cookie non persistente sono basati su sessione.

I concetti di timeout e slidingExpiration già illustrati si applicano lo stesso per entrambi i cookie basati su sessione e scadenza. È presente solo una piccola differenza in esecuzione: quando si usa i cookie di scadenza basati slidingTimeout impostato su true, la scadenza del cookie viene aggiornata solo quando più della metà del tempo specificato è trascorso.

È possibile aggiornare i criteri di timeout ticket di autenticazione del nostro sito Web in modo che i ticket timeout dopo un'ora (60 minuti), con una scadenza variabile. Per rendere effettiva questa modifica, aggiornare il file Web. config, aggiunta di un &lt;form&gt; elemento per il &lt;autenticazione&gt; elemento con il markup seguente:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Utilizzando un URL della pagina account di accesso diversi da Login. aspx

Poiché FormsAuthenticationModule reindirizza automaticamente gli utenti non autorizzati alla pagina di accesso, deve sapere URL della pagina di accesso. Questo URL viene specificato dall'attributo loginUrl nel &lt;form&gt; elemento e il valore predefinito è Login. aspx. Se si trasferisce in un sito Web esistente, si potrebbe avere già una pagina di accesso con un URL diverso, che è già stato contrassegnato con un segnalibro e indicizzati dai motori di ricerca. Anziché rinominare la pagina di accesso esistenti a Login. aspx e interrompere i collegamenti e segnalibri degli utenti, è invece possibile modificare l'attributo loginUrl in modo da puntare alla pagina di accesso.

Ad esempio, se la pagina di accesso è stata denominata SignIn.aspx e si trova nella directory degli utenti, si potrebbe scegliere l'impostazione di configurazione loginUrl a ~/Users/SignIn.aspx come segue:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Poiché l'applicazione corrente già dispone di una pagina di accesso denominata Login. aspx, non è necessario specificare un valore personalizzato nel &lt;form&gt; elemento.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Passaggio 2: Uso i ticket di autenticazione form senza cookie

Per impostazione predefinita il sistema di autenticazione form determina se memorizzare il ticket di autenticazione nella raccolta di cookie o incorporarli nell'URL in base all'agente utente di visitare il sito. Tutti i browser desktop tradizionali, ad esempio Internet Explorer, Firefox, Opera e Safari, i cookie di supporto, ma non tutti i dispositivi portatili.

I criteri di cookie usato dal sistema di autenticazione form dipendono dall'impostazione senza cookie nel &lt;form&gt; elemento che può essere assegnato uno dei quattro valori:

- UseCookies - specifica che verranno utilizzati sempre i ticket di autenticazione basata su cookie.
- UseUri - indica che i ticket di autenticazione basata su cookie non verranno mai utilizzati.
- Rilevamento automatico - se il profilo di dispositivo non supporta i cookie, i ticket di autenticazione basata su cookie non vengono usati; Se il profilo del dispositivo li supporta, viene utilizzato un meccanismo di probe per determinare se i cookie siano abilitati.
- UseDeviceProfile - impostazione predefinita. Usa i ticket di autenticazione basata su cookie, solo se il profilo di dispositivo li supporta. Non viene usato alcun meccanismo di probe.

Le impostazioni di rilevamento automatico e UseDeviceProfile si basano su un *profilo di dispositivo* nel determinare se utilizzare i ticket di autenticazione basata su cookie o senza cookie. ASP.NET gestisce un database di vari tipi di dispositivi e le relative funzionalità, ad esempio se supportano o meno i cookie, quale versione di JavaScript supportano e così via. Ogni volta che un dispositivo richiede una pagina web da un server web invia lungo una *agente utente* intestazione HTTP che identifica il tipo di dispositivo. ASP.NET associa automaticamente la stringa agente utente fornito con il profilo corrispondente specificato nel relativo database.

> [!NOTE]
> Questo database delle funzionalità del dispositivo viene archiviato in un numero di file XML che rispettano il [schema di File di definizione Browser](https://msdn.microsoft.com/library/ms228122.aspx). I file del profilo di dispositivo predefinite si trovano nella cartella % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. È anche possibile aggiungere file personalizzati all'App dell'applicazione\_cartella browser. Per altre informazioni, vedere [How To: rileva i tipi di Browser in ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Poiché l'impostazione predefinita è UseDeviceProfile, i ticket di autenticazione form senza cookie verranno utilizzati quando si visita il sito da un dispositivo con profilo segnala che non supporta i cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Codifica il Ticket di autenticazione nell'URL

I cookie sono un mezzo naturale per l'inclusione di informazioni dal browser in ciascuna richiesta per un particolare sito Web, motivo per cui le impostazioni di autenticazione form predefinito usano se il dispositivo visitando supportarle. Se i cookie non sono supportati, è necessario utilizzare un metodo alternativo per passare il ticket di autenticazione dal client al server. Una soluzione alternativa comune utilizzata in ambienti senza cookie consiste nel codificare i dati del cookie nell'URL.

Il modo migliore per vedere come tali informazioni possono essere incorporate all'interno dell'URL è per forzare il sito per utilizzare i ticket di autenticazione senza cookie. A questo scopo, impostare l'impostazione di configurazione senza cookie a UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Dopo aver apportato questa modifica, visitare il sito tramite un browser. Quando si visita come utente anonimo, l'URL avrà un aspetto esattamente come in precedenza. Ad esempio, quando si visita la pagina default. aspx barra degli indirizzi del browser Mostra l'URL seguente:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Tuttavia, dopo l'accesso, il ticket di autenticazione form è incorporato nell'URL. Ad esempio, dopo aver visitato la pagina di accesso e accedere come Sam, sto torna alla pagina default. aspx, ma questa volta l'URL è:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Il ticket di autenticazione form è stato incorporato all'interno dell'URL. La stringa (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) rappresenta le informazioni sul ticket di autenticazione con codifica esadecimale e sono gli stessi dati che sono in genere archiviati all'interno di un cookie.

Affinché i ticket di autenticazione senza cookie a funzionare, il sistema deve codificare tutti gli URL nella pagina per includere i dati del ticket di autenticazione, in caso contrario, il ticket di autenticazione andranno perso quando l'utente fa clic su un collegamento. Fortunatamente, questa logica di incorporamento viene eseguita automaticamente. Per illustrare questa funzionalità, aprire la pagina default. aspx e aggiungere un controllo collegamento ipertestuale, impostandone le proprietà di testo e Instrumentedlink prova collegamento e SomePage.aspx, rispettivamente. Non è importante che realtà non vi è una pagina nel progetto denominato SomePage.aspx.

Salvare le modifiche apportate a default. aspx e quindi si accede tramite un browser. Accedere al sito in modo che il ticket di autenticazione form è incorporato nell'URL. Successivamente, da default. aspx, fare clic sul collegamento prova collegamento. Cosa è successo? Se non esiste alcuna pagina denominato SomePage.aspx, poi si è verificato un errore 404, ma che non è importante in questo caso. Concentrarsi invece sulla barra degli indirizzi del browser. Si noti che include il ticket di autenticazione form nell'URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

SomePage.aspx l'URL del collegamento è stata convertita automaticamente in un URL che è incluso il ticket di autenticazione - è stato necessario scrivere codice! Il ticket di autenticazione form verrà automaticamente incorporato nell'URL per tutti i collegamenti ipertestuali che non iniziano con `http://` o `/`. Non è importante se il collegamento ipertestuale viene visualizzato in una chiamata a Response. Redirect, in un controllo collegamento ipertestuale o in un elemento ancoraggio HTML (ad esempio, `<a href="...">...</a>`). Fino a quando l'URL non è simile `http://www.someserver.com/SomePage.aspx` o `/SomePage.aspx`, il ticket di autenticazione form verrà incorporato per noi.

> [!NOTE]
> I ticket di autenticazione form senza cookie rispettano gli stessi criteri di timeout come i ticket di autenticazione basata su cookie. Tuttavia, i ticket di autenticazione senza cookie sono più soggetti a attacchi di tipo replay poiché il ticket di autenticazione è incorporato direttamente nell'URL. Si immagini un utente visita un sito Web, effettua l'accesso e quindi Incolla l'URL in un messaggio di posta elettronica a un collega. Se il collega fa clic sul collegamento prima che venga raggiunta la scadenza, essi verrà registrati dell'utente che ha inviato il messaggio di posta elettronica.


## <a name="step-3-securing-the-authentication-ticket"></a>Passaggio 3: Proteggere il Ticket di autenticazione

Il ticket di autenticazione form viene trasmesso in rete in un cookie o incorporati direttamente all'interno dell'URL. Oltre alle informazioni di identità, il ticket di autenticazione può includere anche dati utente (come si vedrà in passaggio 4). Di conseguenza, è importante che i dati del ticket sono crittografati da occhi indiscreti e, ancora più importante, che il sistema di autenticazione form in grado di garantire che il ticket non è stato manomesso.

Per garantire la privacy dei dati del ticket, il sistema di autenticazione form possibile crittografare i dati del ticket. Crittografare i dati del ticket invia informazioni potenzialmente riservate via cavo in testo normale.

Per garantire l'autenticità del ticket, è necessario il sistema di autenticazione form *convalidare* il ticket. La convalida è l'atto di garantire che una particolare porzione di dati non è stata modificata e viene eseguita tramite un  *[il codice di autenticazione (MAC) del messaggio](http://en.wikipedia.org/wiki/Message_authentication_code)*. In breve, il MAC è una piccola parte di informazioni che identificano i dati che deve essere convalidata (in questo caso, il ticket). Se i dati rappresentati da MAC vengono modificati, quindi il MAC e i dati non corrisponderanno. Inoltre, è difficile calcoli complessi a un pirata informatico sia modificare i dati e generare il proprio MAC per comunicare con i dati modificati.

Durante la creazione (o la modifica) un ticket, il sistema di autenticazione form crea un computer MAC e lo collega a dati del ticket. Quando arriva una richiesta successiva, il sistema di autenticazione form confronta i dati di MAC e ticket per convalidare l'autenticità dei dati del ticket. La figura 3 illustra questo flusso di lavoro graficamente.


[![Autenticità del Ticket viene garantita tramite un MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Figura 03**: autenticità del Ticket è garantito tramite un MAC ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Le misure di sicurezza vengono applicate al ticket di autenticazione dipende l'impostazione della protezione la &lt;form&gt; elemento. L'impostazione di protezione può essere assegnata a uno dei tre valori seguenti:

- All - il ticket viene crittografato e contemporaneamente una firma digitale (l'impostazione predefinita).
- Si applica la crittografia - solo la crittografia, non viene generato alcun MAC.
- Nessuno - il ticket non viene crittografato né firmato digitalmente.
- Convalida: un MAC viene generata, ma i dati del ticket viene inviati in rete in testo normale.

Microsoft consiglia di usare l'impostazione di tutti i.

### <a name="setting-the-validation-and-decryption-keys"></a>Impostazione di convalida e le chiavi di decrittografia

La crittografia e hash di algoritmi usati dal sistema di autenticazione form per crittografare e convalidare il ticket di autenticazione sono personalizzabili tramite il [ &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx) in Web. config. Tabella 2 descrive le &lt;machineKey&gt; degli attributi dell'elemento e i relativi valori possibili.

| **Attributo** | **Descrizione** |
| --- | --- |
| decrittografia | Indica l'algoritmo utilizzato per la crittografia. Questo attributo può avere uno dei quattro valori seguenti: - Auto - impostazione predefinita. Determina l'algoritmo in base alla lunghezza dell'attributo decryptionKey. -AES - Usa il [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmo. -DES - Usa il [Data Encryption Standard (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) questo algoritmo viene considerato dal punto di vista debole e non deve essere utilizzato. -3DES - Usa il [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmo, che applica l'algoritmo DES tre volte. |
| decryptionKey | La chiave privata usata dall'algoritmo di crittografia. Questo valore deve essere una stringa esadecimale di lunghezza appropriata (in base al valore di decrittografia), genera automaticamente o il valore aggiunto, IsolateApps. Aggiunta IsolateApps indica ad ASP.NET di usare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |
| convalida | Indica l'algoritmo utilizzato per la convalida. Questo attributo può avere uno dei quattro valori seguenti: - AES - Usa l'algoritmo Advanced Encryption Standard (AES). -MD5 - Usa il [Message-Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmo. -SHA1 - Usa il [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmo (predefinito). -3DES - Usa l'algoritmo Triple DES. |
| validationKey | La chiave privata usata dall'algoritmo di convalida. Questo valore deve essere una stringa esadecimale di lunghezza appropriata (in base al valore nella convalida), genera automaticamente o il valore aggiunto, IsolateApps. Aggiunta IsolateApps indica ad ASP.NET di usare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |

**Tabella 2**: il &lt;machineKey&gt; degli attributi dell'elemento

Una discussione approfondita di queste opzioni di crittografia e convalida e dei vantaggi e svantaggi dei vari algoritmi, esula dall'ambito di questa esercitazione. Per un'analisi approfondita esaminare questi problemi, incluse informazioni aggiuntive su quali algoritmi di crittografia e convalida da usare, quali le lunghezze di chiave da usare e il modo migliore per generare queste chiavi, fare riferimento a *Professional ASP.NET 2.0 Security, l'appartenenza e la gestione dei ruoli* .

Per impostazione predefinita, le chiavi usate per la crittografia e convalida vengono generate automaticamente per ogni applicazione e queste chiavi vengono archiviate in Autorità di protezione locale (LSA). In breve, le impostazioni predefinite garantiscono che le chiavi univoche di un server web dal server web e dell'applicazione per applicazione. Di conseguenza, questo comportamento predefinito non funzionerà per i due scenari seguenti:

- **Web farm** : in un [farm web](http://en.wikipedia.org/wiki/Web_farm) scenario, una singola applicazione web è ospitato in più server web per ottenere scalabilità e ridondanza. Ogni richiesta in ingresso viene inviato a un server nella farm, vale a dire che tutta la durata della sessione dell'utente, server diversi può essere utilizzato per gestire il suo diverse richieste. Di conseguenza, ogni server deve usare le stesse chiavi di crittografia e convalida in modo che il ticket di autenticazione form creato, crittografato e convalidato in un server può essere decrittografato e convalidato in un altro server nella farm.
- **Tra la condivisione delle applicazioni Ticket** -un singolo server web può ospitare più applicazioni ASP.NET. Se è necessario per queste applicazioni diverse di condividere un ticket di autenticazione form singolo, è fondamentale che le relative chiavi di crittografia e convalida abbinare.

Quando si lavora in una web farm impostazione o condividere i ticket di autenticazione tra più applicazioni sullo stesso server, è necessario configurare il &lt;machineKey&gt; elemento nelle applicazioni interessate in modo che i decryptionKey e i valori di validationKey abbinare.

Mentre nessuno dei due scenari descritti è valido per l'applicazione di esempio, è ancora possibile specificare decryptionKey esplicite e i valori di validationKey e definire gli algoritmi da usare. Aggiungere un &lt;machineKey&gt; impostazione nel file Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Per ulteriori informazioni consultare [procedura: configurare MachineKey di ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> I valori decryptionKey e validationKey sono stati ricavati dalla [Steve Gibson](http://www.grc.com/stevegibson.htm)del [pagina web password perfetto](https://www.grc.com/passwords.htm), che genera i 64 caratteri esadecimali casuali su ogni visita di pagina. Per ridurre la probabilità che queste chiavi facciano strada le applicazioni di produzione, sono invitati a sostituire le chiavi precedenti con quelli generati casualmente dalla pagina password perfetto.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Passaggio 4: L'archiviazione dei dati utente aggiuntivi nel Ticket

Molte applicazioni web su cui visualizzare informazioni o visualizzazione della pagina basata sull'utente attualmente connesso. Ad esempio, una pagina web potrebbe visualizzare il nome dell'utente e la data che ha effettuato l'ultimo accesso nell'angolo superiore di ogni pagina. Il ticket di autenticazione form archivia il nome utente dell'utente attualmente connesso, ma quando qualsiasi altra informazione è necessaria, la pagina deve passare nell'archivio utente, in genere un database - per cercare le informazioni non archiviate nel ticket di autenticazione.

Con un po' di codice che è possibile archiviare informazioni aggiuntive sull'utente nel ticket di autenticazione form. Tali dati possono essere espressi tramite il [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)del [UserData proprietà](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Si tratta di un ideale per contenere piccole quantità di informazioni sull'utente che è comunemente necessari. Il valore specificato in usato per UserData proprietà è inclusa come parte del cookie di ticket di autenticazione e, come gli altri campi ticket, viene crittografata e convalidata basata sulla configurazione del sistema di autenticazione form. Per impostazione predefinita, UserData è una stringa vuota.

Per archiviare i dati utente del ticket di autenticazione, è necessario scrivere un po' di codice nella pagina di accesso che recupera le informazioni specifiche dell'utente e la archivia nel ticket. Poiché UserData è una proprietà di tipo stringa, i dati archiviati in essa devono essere serializzati correttamente come una stringa. Ad esempio, si supponga che l'archivio utente inclusi data di nascita di ogni utente e il nome del loro datore di lavoro e si desidera archiviare i valori delle due proprietà del ticket di autenticazione. È stato possibile serializzare questi valori in una stringa concatenando la data dell'utente della stringa di nascita con una barra verticale (|), seguito dal nome del datore di lavoro. Per un utente nato nel 15 agosto 1974 che funziona per Northwind Traders, è necessario assegnare la proprietà UserData la stringa: 1974-08-15 | Northwind Traders.

Ogni volta che è necessario accedere ai dati archiviati nel ticket, è possibile eseguire da selezionandola FormsAuthenticationTicket della richiesta corrente e la deserializzazione della proprietà UserData. Nel caso la data di nascita e il datore di lavoro di esempio di nome, si potrebbe suddividere la stringa UserData in due sottostringhe in base al delimitatore (|).


[![Informazioni aggiuntive sull'utente possono essere archiviati nel Ticket di autenticazione](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Figura 04**: ulteriori utente le informazioni possono essere archiviati nel Ticket di autenticazione ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>La scrittura di informazioni per UserData

Sfortunatamente, aggiunta di informazioni specifiche dell'utente in un ticket di autenticazione form non è così semplice come ci si aspetterebbe. La proprietà UserData della classe FormsAuthenticationTicket è di sola lettura e può essere specificata solo tramite il costruttore di classe oggetto FormsAuthenticationTicket. Quando si specifica la proprietà UserData nel costruttore, è inoltre necessario fornire il ticket's altri valori: il nome utente, la data di emissione, la scadenza e così via. Quando la pagina di accesso abbiamo creato nell'esercitazione precedente, questa è stata tutti gestita automaticamente dalla classe FormsAuthentication. Quando si aggiunge UserData FormsAuthenticationTicket, è necessario scrivere codice per eseguire la replica di gran parte della funzionalità già fornita dalla classe FormsAuthentication.

Esaminiamo il codice necessario per l'utilizzo di dati utente aggiornando la pagina aspx per registrare informazioni aggiuntive sull'utente per il ticket di autenticazione. Seguire le che l'archivio utente contiene informazioni su azienda lavora per l'utente e qualifica professionale, e che si desidera acquisire queste informazioni nel ticket di autenticazione. Aggiornare gestore dell'evento LoginButton Click della pagina aspx in modo che il codice simile al seguente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Esaminiamo questo codice di una riga alla volta. Il metodo inizia definendo quattro matrici di stringhe: gli utenti, password, companyName e titleAtCompany. Queste matrici contengono i nomi utente, password, i nomi di società e i titoli degli account utente nel sistema, di cui sono disponibili tre: Scott Jisun e Sam. In un'applicazione reale, questi valori potrebbero eseguire query dall'archivio dell'utente, non impostate come hardcoded nel codice sorgente della pagina.

Nell'esercitazione precedente, se le credenziali specificate non sono valide viene semplicemente chiamato FormsAuthentication (UserName.Text, RememberMe.Checked), che i seguenti passaggi:

1. Creazione di form ticket di autenticazione
2. Ha scritto il ticket per l'archivio appropriato. Per i ticket di autenticazione basata su cookie, viene usato l'insieme di cookie del browser; per i ticket di autenticazione senza cookie, i dati del ticket viene serializzati nell'URL
3. Reindirizzato l'utente alla pagina appropriata

Questi passaggi vengono replicati nel codice precedente. In primo luogo, la stringa che verranno infine archiviati nella proprietà UserData è formata combinando il nome della società e il titolo, che delimitano i due valori con un carattere di barra verticale (|).

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

Successivamente, il metodo viene richiamato, FormsAuthentication.GetAuthCookie che crea il ticket di autenticazione, crittografa la convalida in base alle impostazioni di configurazione e lo inserisce in un oggetto HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked);

Per usare il FormAuthenticationTicket incorporato all'interno del cookie, dobbiamo chiamare la classe di FormAuthentication [decrittografare metodo](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), passando il valore del cookie.

Ticket FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value);

Viene quindi creata una *nuovo* istanza FormsAuthenticationTicket basato sui valori dell'oggetto FormsAuthenticationTicket esistente. Tuttavia, questo nuovo ticket include le informazioni specifiche dell'utente (userDataString).

Oggetto FormsAuthenticationTicket newTicket = nuovo FormsAuthenticationTicket (ticket. Versione, ticket. Nome del ticket. Data di rilascio, ticket. Scadenza, ticket. IsPersistent, userDataString);

Abbiamo quindi crittografare (e convalidare) la nuova istanza di oggetto FormsAuthenticationTicket chiamando il [crittografare metodo](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)e reinserire i dati crittografati (e convalidati) nel authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Infine, authCookie viene aggiunto alla raccolta Cookies e viene chiamato il metodo GetRedirectUrl per determinare la pagina appropriata per inviare all'utente.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Tutto il codice è necessaria perché la proprietà UserData è di sola lettura e la classe FormsAuthentication non fornisce alcun metodo per specificare le informazioni di UserData nei relativi metodi GetAuthCookie, SetAuthCookie o RedirectFromLoginPage.

> [!NOTE]
> Il codice che viene esaminato solo archivia informazioni specifiche dell'utente in un ticket di autenticazione basata su cookie. Le classi responsabili per la serializzazione il ticket di autenticazione form all'URL sono interne a .NET Framework. In breve, è possibile archiviare i dati utente in un ticket di autenticazione form senza cookie.


### <a name="accessing-the-userdata-information"></a>Accesso alle informazioni UserData

A questo punto il nome della società e titolo di ogni utente viene archiviato nella proprietà UserData del ticket di autenticazione form all'accesso. Queste informazioni sono accessibili dal ticket di autenticazione in qualsiasi pagina senza richiedere una corsa all'archivio dell'utente. Per illustrare come queste informazioni possono essere recuperate dalla proprietà UserData, aggiorniamo default. aspx in modo che il messaggio di benvenuto include non solo il nome dell'utente, ma anche l'azienda che lavorano per e titolo.

Attualmente, default. aspx contiene un pannello AuthenticatedMessagePanel con un controllo etichetta denominato WelcomeBackMessage. Questo pannello viene visualizzato solo agli utenti autenticati. Aggiornare il codice nella pagina del default. aspx\_caricare il gestore dell'evento in modo che risulti simile al seguente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Se Request.IsAuthenticated è true e quindi del WelcomeBackMessage testo viene innanzitutto impostata su Bentornato, *username*. Quindi, la proprietà di User. Identity viene eseguito il cast a un oggetto FormsIdentity in modo che sia possibile accedere FormsAuthenticationTicket sottostante. Dopo aver ottenuto l'oggetto FormsAuthenticationTicket, abbiamo deserializzare la proprietà UserData nel nome della società e il titolo. Questa operazione viene eseguita dividendo la stringa sul carattere di barra verticale. Il nome della società e il titolo viene quindi visualizzati nell'etichetta WelcomeBackMessage.

Figura 5 mostra una schermata di questa visualizzazione in azione. Accedere come Scott Visualizza un messaggio di back-benvenuto che include aziendale e il titolo di Scott.


[![Vengono visualizzati l'attualmente registrati dell'utente aziendale e titolo](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Figura 05**: vengono visualizzati il attualmente registrati dell'utente aziendale e Title ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Proprietà di UserData del ticket di autenticazione viene utilizzata come una cache per l'archivio dell'utente. Ad esempio una cache qualsiasi, deve essere aggiornato quando vengono modificati i dati sottostanti. Ad esempio, se è presente una pagina web da cui gli utenti possono aggiornare il proprio profilo, è necessario aggiornare i campi memorizzato nella cache nella proprietà UserData in modo da riflettere le modifiche apportate dall'utente.


## <a name="step-5-using-a-custom-principal"></a>Passaggio 5: Uso di un'entità personalizzata

Ogni richiesta in ingresso FormsAuthenticationModule tenta di autenticare l'utente. Se è presente un ticket di autenticazione non scadute, FormsAuthenticationModule assegna la proprietà HttpContext. User a un nuovo oggetto GenericPrincipal. Questo oggetto GenericPrincipal ha un'identità del tipo FormsIdentity, che include un riferimento al ticket di autenticazione form. La classe di oggetti GenericPrincipal contiene bare funzionalità minime necessarie per una classe che implementa IPrincipal: è presente solo una proprietà Identity e un metodo IsInRole.

L'oggetto principal ha due compiti: per indicare i ruoli a cui appartiene l'utente e per fornire informazioni di identità. Questa operazione viene eseguita tramite IsInRole dell'interfaccia IPrincipal (*roleName*) metodo e proprietà Identity, rispettivamente. La classe di oggetti GenericPrincipal consente di una matrice di stringhe di nomi di ruolo è possibile specificare tramite il relativo costruttore; relativo IsInRole (*roleName*) metodo semplice verifica se il valore passato in *roleName* esiste all'interno della matrice di stringa. Quando FormsAuthenticationModule crea l'oggetto GenericPrincipal, passa una matrice di stringa vuota al costruttore dell'oggetto GenericPrincipal. Di conseguenza, qualsiasi chiamata a IsInRole restituirà sempre false.

La classe di oggetti GenericPrincipal soddisfa le esigenze per la maggior parte degli scenari di autenticazione basata su form in cui non vengono utilizzati i ruoli. Per i casi in cui non è sufficiente la gestione di ruolo predefinita o quando è necessario associare un oggetto IIdentity personalizzato con l'utente, è possibile creare un oggetto di IPrincipal personalizzato durante il flusso di lavoro di autenticazione e assegnarla alla proprietà HttpContext. User.

> [!NOTE]
> Come si vedrà in futuro le esercitazioni, quando ASP. Framework di ruoli del .NET è abilitata viene creato un oggetto personalizzato dell'entità di tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) e sovrascrive l'oggetto GenericPrincipal creati dall'autenticazione form. Ciò avviene per personalizzare il metodo IsInRole dell'entità all'interfaccia con l'API del framework dei ruoli.


Poiché è stato non occupa noi stessi ruoli ancora, l'unico motivo per cui che è stato per la creazione di un'entità personalizzata a questo punto, è possibile associare un oggetto IIdentity personalizzato all'entità. Nel passaggio 4 è stato illustrato l'archiviazione delle informazioni utente aggiuntive nella proprietà UserData del ticket di autenticazione, in particolare il nome dell'utente aziendale e qualifica professionale. Tuttavia, le informazioni di UserData sono solo accessibili tramite il ticket di autenticazione e quindi solo sotto forma di stringa serializzata, vale a dire che ogni volta che si desidera visualizzare le informazioni utente archiviate nel ticket è necessario analizzare la proprietà UserData.

L'esperienza di sviluppo migliorare creando una classe che implementa IIdentity e include le proprietà di CompanyName e il titolo. In questo modo, uno sviluppatore può accedere a nome della società dell'utente attualmente connesso e titolo direttamente tramite le proprietà di CompanyName e titolo senza necessario conoscere le modalità per analizzare la proprietà UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Creazione di identità personalizzato e le classi di entità

Per questa esercitazione, è possibile creare gli oggetti principal e identity personalizzati nell'App\_cartella del codice. Iniziare aggiungendo un'App\_cartella nel progetto di codice: pulsante destro del mouse sul nome del progetto in Esplora soluzioni, selezionare l'opzione Aggiungi cartella ASP.NET e scegliere l'App\_codice. L'App\_cartella del codice è una speciale cartella ASP.NET che contiene file di classe specifico per il sito Web.

> [!NOTE]
> L'App\_cartella del codice deve essere usata solo quando la gestione del progetto tramite il modello di progetto sito Web. Se si usa la [modello di progetto applicazione Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), creare una cartella standard e aggiungervi le classi. Ad esempio, è possibile aggiungere una nuova cartella denominata classi e inserirvi il codice.


Successivamente, aggiungere due nuovi file di classe per l'App\_cartella del codice, CustomIdentity.cs denominata uno e uno denominato CustomPrincipal.cs.


[![Aggiungere tutte le classi CustomPrincipal CustomIdentity al progetto](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Figura 06**: aggiungere tutte le classi CustomPrincipal CustomIdentity a un progetto ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


La classe CustomIdentity è responsabile dell'implementazione dell'interfaccia IIdentity, che definisce le proprietà AuthenticationType IsAuthenticated e nome. Oltre a queste proprietà richieste, siamo interessati a esporre la sottostante ticket di autenticazione form, nonché le proprietà per nome della società e il titolo dell'utente. Immettere il codice seguente nella classe CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Si noti che la classe include una variabile membro FormsAuthenticationTicket (\_ticket) e che queste informazioni ticket devono essere fornite tramite il costruttore. Questi dati ticket viene usati nel restituire il nome dell'identità; la proprietà UserData viene analizzata per restituire i valori per le proprietà di CompanyName e il titolo.

Successivamente, creare la classe CustomPrincipal. Poiché non si è interessati ai ruoli a questo punto, il costruttore della classe CustomPrincipal accetta solo un oggetto CustomIdentity. il metodo IsInRole restituisce sempre false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Assegnazione di un oggetto CustomPrincipal al contesto di sicurezza della richiesta in ingresso

È ora disponibile una classe che estende la specifica di IIdentity predefinita per includere le proprietà di CompanyName e il titolo, nonché una classe di entità personalizzata che usa l'identità personalizzata. Si è pronti per eseguire l'istruzione della pipeline ASP.NET e si assegna l'oggetto entità personalizzata al contesto di sicurezza della richiesta in ingresso.

La pipeline ASP.NET accetta una richiesta in ingresso e lo elabora mediante una serie di passaggi. A ogni passaggio, viene generato un evento specifico, rendendo possibile per gli sviluppatori di interagire con la pipeline ASP.NET e modificare la richiesta in determinati punti nel ciclo di vita. FormsAuthenticationModule, ad esempio, attende che ASP.NET generare il [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), a quel punto ed esamina la richiesta in ingresso per un ticket di autenticazione. Se viene trovato un ticket di autenticazione, un oggetto GenericPrincipal viene creato e assegnato alla proprietà HttpContext. User.

Dopo l'evento, AuthenticateRequest della pipeline ASP.NET genera il [evento PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), che è in cui è possibile sostituire l'oggetto GenericPrincipal creato da FormsAuthenticationModule con un'istanza di nostro Oggetto CustomPrincipal. Figura 7 illustra questo flusso di lavoro.


[![L'oggetto GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Figura 07**: l'oggetto GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Per eseguire codice in risposta a un evento di pipeline ASP.NET, è possibile creare il gestore eventi appropriato in Global. asax o creare un modulo HTTP. Per questa esercitazione è possibile creare il gestore dell'evento in Global. asax. Iniziare aggiungendo Global. asax per il sito Web. Fare doppio clic sul nome del progetto in Esplora soluzioni e aggiungere un elemento di tipo classe di applicazione globale denominato Global. asax.


[![Aggiungere un File Global. asax per il sito Web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Figura 08**: aggiungere un File Global. asax per il sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


Il modello di Global. asax predefinito include i gestori eventi per un numero di eventi della pipeline ASP.NET, tra cui inizio, fine e [evento di errore](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), tra gli altri. È possibile rimuovere questi gestori eventi, come abbiamo non siano necessarie per questa applicazione. L'evento che si è interessati è PostAuthenticateRequest. Aggiornare il file Global. asax in modo che il markup ha un aspetto simile al seguente:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

L'applicazione\_OnPostAuthenticateRequest metodo viene eseguito ogni volta che il runtime ASP.NET genera l'evento PostAuthenticateRequest, che viene eseguita una volta per ogni richiesta di pagina in ingresso. Il gestore dell'evento inizia con un controllo per verificare se l'utente viene autenticato e autenticata tramite autenticazione basata su form. In questo caso, un nuovo oggetto CustomIdentity viene creato e passato ticket di autenticazione della richiesta corrente nel relativo costruttore. In seguito, un oggetto CustomPrincipal è creato e passato l'oggetto CustomIdentity appena creata nel relativo costruttore. Infine, il contesto di sicurezza della richiesta corrente viene assegnato all'oggetto CustomPrincipal appena creato.

Si noti che l'ultimo passaggio - associazione oggetto CustomPrincipal con contesto di sicurezza della richiesta - assegna l'entità a due proprietà: HttpContext. User e thread. CurrentPrincipal. Queste due assegnazioni sono necessarie a causa della modalità contesti di sicurezza vengono gestiti in ASP.NET. .NET Framework consente di associare un contesto di sicurezza con ogni thread in esecuzione. Queste informazioni sono disponibili come un oggetto di IPrincipal tramite il [oggetto Thread](https://msdn.microsoft.com/library/system.threading.thread.aspx)del [proprietà CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Che cos'è un chiaro è che ASP.NET ha le proprie informazioni di contesto di sicurezza (HttpContext. User).

In alcuni scenari, la proprietà thread. CurrentPrincipal viene esaminata per determinare il contesto di sicurezza. in altri scenari, viene usato HttpContext. User. Ad esempio, sono disponibili funzionalità di sicurezza in .NET che consentono agli sviluppatori di dichiarare in modo dichiarativo ciò che gli utenti o ruoli è possono creare un'istanza di una classe o richiamare metodi specifici (vedere [aggiunta di regole di autorizzazione per Business e dati usando i livelli PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Sotto le quinte, queste tecniche dichiarative determinano il contesto di sicurezza tramite la proprietà thread. CurrentPrincipal.

In altri scenari, viene utilizzata la proprietà HttpContext. User. Ad esempio, nell'esercitazione precedente abbiamo utilizzato questa proprietà per visualizzare il nome utente dell'utente attualmente connesso. Chiaramente, quindi, è fondamentale che le informazioni sul contesto di sicurezza nelle proprietà del thread. CurrentPrincipal e HttpContext abbinare.

Il runtime ASP.NET sincronizzerà automaticamente i valori delle proprietà per noi. Tuttavia, questa sincronizzazione si verifica dopo l'evento AuthenticateRequest, ma *prima di* evento PostAuthenticateRequest. Di conseguenza, quando si aggiunge un'entità personalizzata nell'evento PostAuthenticateRequest dobbiamo essere certi assegnare manualmente il thread. CurrentPrincipal oppure il thread. CurrentPrincipal e HttpContext sarà sincronizzato. Vedere [vs Context. User. Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) per una discussione più dettagliata su questo problema.

### <a name="accessing-the-companyname-and-title-properties"></a>L'accesso al CompanyName e proprietà titolo

Ogni volta che una richiesta arriva e viene inviata al motore di ASP.NET, l'applicazione\_genererà OnPostAuthenticateRequest gestore dell'evento in Global. asax. Se la richiesta è stata autenticata correttamente da FormsAuthenticationModule, il gestore dell'evento creerà un nuovo oggetto CustomPrincipal con un oggetto CustomIdentity basato su ticket di autenticazione form. Con questa logica posto, è incredibilmente semplice l'accesso alle informazioni sul nome della società e il titolo dell'utente attualmente connesso.

Tornare alla pagina\_gestore di eventi di caricamento in default. aspx, in cui nel passaggio 4 abbiamo scritto codice per recuperare il ticket di autenticazione form e analizzare la proprietà UserData per visualizzare il nome della società e il titolo dell'utente. Con gli oggetti CustomPrincipal e CustomIdentity in uso a questo punto, non è necessario per analizzare i valori dalla proprietà UserData del ticket. In alternativa, è sufficiente ottenere un riferimento all'oggetto CustomIdentity e usarne le proprietà di CompanyName e il titolo:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Riepilogo

In questa esercitazione abbiamo esaminato come personalizzare le impostazioni del sistema di autenticazione form tramite Web. config. È stato illustrato come viene gestita la scadenza del ticket di autenticazione e l'uso di crittografia e convalida le misure di sicurezza per proteggere il ticket da analisi e la modifica. Infine, viene illustrato l'uso di proprietà UserData del ticket di autenticazione per archiviare informazioni aggiuntive sull'utente nel ticket stesso e come usare oggetti principal e identity personalizzati per esporre queste informazioni in modo più semplice.

Questa esercitazione è terminata l'esame dell'autenticazione basata su form in ASP.NET. L'esercitazione successiva avvia nostro viaggio nel framework di appartenenza.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Analisi dettagliata di autenticazione basata su form](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Spiegazione: Autenticazione basata su form in ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Procedura: Proteggere l'autenticazione basata su form in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professionisti ASP.NET 2.0 Security, l'appartenenza e la gestione dei ruoli](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Protezione dei controlli di accesso](https://msdn.microsoft.com/library/ms178346.aspx)
- [Il &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Il &lt;form&gt; (elemento) per &lt;autenticazione&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Il &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [La comprensione dei Ticket di autenticazione form e Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione in video su argomenti contenuti in questa esercitazione

- [Come modificare le proprietà di autenticazione form](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Come configurare e usare l'autenticazione senza Cookie in un'applicazione ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Rilocazione dell'accesso ai moduli ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configurazione della chiave personalizzata di accesso ai moduli](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Aggiungere dati personalizzati al metodo di autenticazione](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usare oggetti Principal personalizzati](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Alicja Maziarz. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Precedente](an-overview-of-forms-authentication-cs.md)
> [Successivo](security-basics-and-asp-net-support-vb.md)
