---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Argomenti avanzati (c#) e configurazione dell'autenticazione form | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione verrà esaminare le varie impostazioni di autenticazione form e informazioni su come modificarle tramite l'elemento di form. Ciò comporterà un dettagliate...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: 58d87bd6211ae1b1eea227d34c001239efcf5f1d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961399"
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Configurazione dell'autenticazione form e argomenti avanzati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> In questa esercitazione verrà esaminare le varie impostazioni di autenticazione form e informazioni su come modificarle tramite l'elemento di form. Ciò comporterà un'analisi approfondita del valore di timeout del ticket di autenticazione form, utilizzando una pagina di accesso con un URL personalizzato (ad esempio SignIn.aspx anziché Login. aspx), nonché i ticket di autenticazione form senza cookie di personalizzazione.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](an-overview-of-forms-authentication-cs.md) abbiamo esaminato i passaggi necessari per l'implementazione di autenticazione basata su form in un'applicazione ASP.NET, dalla specifica delle impostazioni di configurazione in Web. config per la creazione di un log nella pagina per visualizzare diversi contenuto per gli utenti anonimi e autenticati. Tenere presente che è stato configurato il sito Web per l'utilizzo dell'autenticazione form impostando l'attributo mode del &lt;autenticazione&gt; elemento a un form. Il &lt;authentication&gt; elemento può includere facoltativamente un &lt;form&gt; elemento figlio, tramite il quale si può specificare un'ampia gamma di impostazioni di autenticazione form.

In questa esercitazione verrà esaminare le varie impostazioni di autenticazione form e come modificarli tramite il &lt;form&gt; elemento. Ciò comporterà un'analisi approfondita del valore di timeout del ticket di autenticazione form, utilizzando una pagina di accesso con un URL personalizzato (ad esempio SignIn.aspx anziché Login. aspx), nonché i ticket di autenticazione form senza cookie di personalizzazione. Verrà inoltre esaminare più da vicino la composizione del ticket di autenticazione form e vedere le precauzioni che ASP.NET utilizza per garantire la sicurezza di ispezione e manomissione dei dati del ticket. Infine, verranno esaminati come archiviare dati aggiuntivi dell'utente nel ticket di autenticazione form e come modellare i dati tramite un oggetto entità personalizzato.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Passaggio 1: Esame il &lt;form&gt; le impostazioni di configurazione

Il sistema di autenticazione form in ASP.NET offre una serie di impostazioni di configurazione che possono essere personalizzati in base dall'applicazione. Ciò include le impostazioni, ad esempio: la durata dell'autenticazione form di concessione ticket; il tipo di protezione viene applicato al ticket; in quali autenticazione cookieless condizioni vengono utilizzati i ticket; il percorso alla pagina di accesso; e altre informazioni. Per modificare i valori predefiniti, aggiungere un [ &lt;form&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) come elemento figlio di [ &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx), che specifica le proprietà si desidera personalizzare come attributi XML i valori nel modo seguente:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Tabella 1 sono riepilogate le proprietà che possono essere personalizzate tramite il &lt;form&gt; elemento. Poiché Web. config è un file XML, i nomi di attributo nella colonna sinistra sono distinzione maiuscole/minuscole.


| <strong>Attributo</strong> |                                                                                                                                                                                                                                     <strong>Descrizione</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         senza cookie         |                                                                                                                Questo attributo specifica in quali condizioni il ticket di autenticazione verrà archiviato in un cookie e viene incorporato nell'URL. Valori consentiti sono: UseCookies; UseUri; Rilevamento automatico; e UseDeviceProfile (impostazione predefinita). Passaggio 2 viene esaminata questa impostazione in modo più dettagliato.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica l'URL che gli utenti vengono reindirizzati a dopo l'accesso dalla pagina di accesso se è presente alcun valore RedirectUrl specificato nella stringa di query. Il valore predefinito è default. aspx.                                                                                                                                                         |
|           dominio           | Quando si utilizzano i ticket di autenticazione basato su cookie, questa impostazione specifica il valore di dominio del cookie. Il valore predefinito è una stringa vuota, che fa sì che il browser da usare il dominio da cui sia stato rilasciato (ad esempio www.yourdomain.com). In questo caso, il cookie verrà <strong>non</strong> da inviare quando apportato richiede di sottodomini, ad esempio admin.yourdomain.com. Se si desidera che il cookie deve essere passato a tutti i sottodomini che è necessario personalizzare l'attributo di dominio se è impostato su NomeDominio. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Valore booleano che indica se gli utenti autenticati sono memorizzati quando reindirizzato all'URL in altre applicazioni web nello stesso server. Il valore predefinito è false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      L'URL della pagina di accesso. Il valore predefinito è Login. aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Quando si usano i ticket di autenticazione basata su cookie, il nome del cookie. Il valore predefinito è. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Quando si utilizzano i ticket di autenticazione basato su cookie, questa impostazione specifica l'attributo di percorso del cookie. L'attributo path consente a uno sviluppatore limitare l'ambito di un cookie a una gerarchia di directory specifico. Il valore predefinito è /, che informa il browser invii il cookie di ticket di autenticazione a qualsiasi richiesta eseguita al dominio.                                                                              |
|         protezione         |                                                                                                                                            Indica quali tecniche usate per proteggere il ticket di autenticazione form. I valori consentiti sono: tutte (predefinito); Crittografia; None; e la convalida. Queste impostazioni sono illustrate in dettaglio nel passaggio 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Valore booleano che indica se è necessaria una connessione SSL per trasmettere il cookie di autenticazione. Il valore predefinito è false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Valore booleano che indica che se il timeout del cookie di autenticazione viene reimpostato ogni volta che l'utente visita il sito durante una singola sessione. Il valore predefinito è true. I criteri di timeout ticket di autenticazione verrà discusso più dettagliatamente nella specifica sezione di valore di Timeout del Ticket.                                                                                                 |
|          timeout           |                                                                                                                               Specifica il tempo, espresso in minuti, trascorso il quale il cookie di ticket di autenticazione scade. Il valore predefinito è 30. I criteri di timeout ticket di autenticazione verrà discusso più dettagliatamente nella specifica sezione di valore di Timeout del Ticket.                                                                                                                               |

**Tabella 1**: un riepilogo del &lt;form&gt; attributi dell'elemento

In ASP.NET 2.0 e versioni successive, l'impostazione predefinita i valori di autenticazione form sono hardcoded nella classe FormsAuthenticationConfiguration in .NET Framework. Su base dall'applicazione nel file Web. config, è necessario applicare le modifiche. Questo comportamento è diverso da ASP.NET 1.x, in cui i valori di autenticazione moduli predefiniti sono stati archiviati nel file Machine. config (e pertanto può essere modificati mediante la modifica di Machine. config). Mentre per l'argomento di ASP.NET 1.x, vale la pena di menzionare che un numero di impostazioni di sistema di autenticazione form dispone di valori predefiniti diversi in ASP.NET 2.0 e oltre a ASP.NET 1.x. Se si esegue la migrazione dell'applicazione da un ambiente 1.x ASP.NET, è importante essere a conoscenza di queste differenze. Consultare [il &lt;form&gt; documentazione tecnica elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) per un elenco delle differenze.

> [!NOTE]
> Diverse impostazioni di autenticazione form, ad esempio il timeout, dominio e il percorso, specificano i dettagli per il cookie di ticket di autenticazione form risultante. Per ulteriori informazioni sul cookie, il loro funzionamento e le relative proprietà diverse, leggere [in questa esercitazione i cookie](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Se si specifica il valore di Timeout del Ticket

Il ticket di autenticazione form è un token che rappresenta un'identità. Con i ticket di autenticazione basata su cookie, questo token è mantenuto in formato di un cookie e inviato al server web per ogni richiesta. Il possesso del token, in pratica, dichiara, sto *username*, si hanno già effettuato l'accesso e viene utilizzato in modo che l'identità dell'utente possono essere memorizzati in pagina visite.

Il ticket di autenticazione form non solo include l'identità dell'utente, ma contiene anche informazioni per aiutare a garantire l'integrità e la sicurezza del token. Dopo tutto, non si nefandi utente non deve essere in grado di creare un token contraffatto o per modificare un token legalmente in qualche modo underhanded.

Un tale bit di informazioni incluse nel ticket è un *scadenza*, ovvero la data e ora il ticket non è più valido. Ogni volta che FormsAuthenticationModule controlla un ticket di autenticazione, assicura che la scadenza del ticket non è ancora passato. In caso affermativo, ignora il ticket e identifica l'utente come anonimo. Tale salvaguardia aiuta a proteggersi da attacchi di tipo replay. Senza una scadenza, se un pirata informatico è stato in grado di ottenere proprio mani nel ticket di autenticazione valido di un utente, ad esempio ottenga l'accesso fisico ai propri computer e rooting tramite i cookie, è stato possibile inviare una richiesta al server con il ticket di autenticazione e ottenere l'accesso. Mentre la scadenza non impedisce di questo scenario, non limita la finestra durante i quali un attacco può avere esito positivo.

> [!NOTE]
> Passaggio 3 dettagli tecniche aggiuntive utilizzate dal sistema di autenticazione form per proteggere il ticket di autenticazione.


Quando si crea il ticket di autenticazione, il sistema di autenticazione form determina la scadenza del consultando l'impostazione di timeout. Come indicato nella tabella 1, il timeout della definizione delle impostazioni predefinite per 30 minuti, vale a dire che, quando viene creato il ticket di autenticazione form di scadenza è impostata su una data e ora nel futuro di 30 minuti.

La scadenza definisce un'ora assoluta in futuro quando scade il ticket di autenticazione form. Ma in genere gli sviluppatori desiderano implementare una scadenza scorrevole, che viene reimpostato ogni volta che l'utente effettua nuovamente richieste del sito. Questo comportamento è determinato dalle impostazioni slidingExpiration. Se impostato su true (impostazione predefinita), ogni volta che un utente viene autenticato FormsAuthenticationModule, consente di aggiornare la scadenza del ticket. Se impostato su false, la scadenza non viene aggiornata a ogni richiesta, causando il ticket scadere esattamente timeout numero di minuti passati quando il ticket è stato inizialmente creato.

> [!NOTE]
> La scadenza archiviata nel ticket di autenticazione è una data assoluta e il valore di ora, ad esempio 2 agosto 2008 11:34 AM, Inoltre, la data e l'ora sono rispetto all'ora locale del server web. Questa decisione progettuale, può avere alcuni effetti collaterali interessanti intorno all'ora legale (DST), vale a dire gli orologi negli Stati Uniti vengono spostati-ahead un'ora (presupponendo che il server web è ospitato in una lingua in cui venga rispettato l'ora legale). Si consideri cosa accadrebbe per un sito Web ASP.NET con una scadenza 30 minuti in prossimità l'ora di inizio dell'ora legale (ovvero alle 2.00). Si supponga che un visitatore accede al sito di 11 marzo 2008 alle 13.00: 55. Ciò provocherebbe un ticket di autenticazione form che scade 11 marzo 2008 alle 2:25 (30 minuti in futuro). Tuttavia, una volta alle 2.00 RTM, l'orologio passa alla 3.00 a causa dell'ora legale. Quando l'utente carica una nuova pagina sei minuti dopo l'accesso (in 3:01 AM), FormsAuthenticationModule rileva che il ticket è scaduto e l'utente viene reindirizzato alla pagina di accesso. Per una discussione più approfondita su questo e altri peculiarità timeout ticket di autenticazione, nonché soluzioni alternative, prelevare una copia di Stefan Schackow *Professional ASP.NET 2.0 sicurezza, l'appartenenza e gestione dei ruoli* (codice ISBN: 978-0-7645-9698-8).


Figura 1 illustra il flusso di lavoro quando slidingExpiration viene impostata su false e timeout è impostato su 30. Si noti che il ticket di autenticazione generato in account di accesso contiene la data di scadenza e questo valore non viene aggiornato nelle richieste successive. Se la classe FormsAuthenticationModule rileva che il ticket è scaduto, lo ignora e gestisce la richiesta come anonimo.


[![Una rappresentazione grafica dei slidingExpiration scadenza quando del Ticket di autenticazione dei form è false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Figura 01**: una rappresentazione grafica dei slidingExpiration scadenza quando del Ticket di autenticazione dei form è false ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


La figura 2 mostra il flusso di lavoro quando slidingExpiration viene impostata su true e il timeout è impostato su 30. Quando viene ricevuta una richiesta autenticata (con un ticket non scadute) viene aggiornata la scadenza del timeout numero di minuti in futuro.


[![Una rappresentazione grafica del Ticket di autenticazione form quando viene soddisfatta slidingExpiration](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Figura 02**: una rappresentazione grafica del Ticket di autenticazione form quando viene soddisfatta slidingExpiration ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Quando si utilizza il ticket di autenticazione basato su cookie (impostazione predefinita), questa discussione diventa più complicata perché i cookie possono trovarsi i propri oggetti scaduti nella specificato. Scadenza di un cookie (o assenza) indica al browser quando il cookie deve essere eliminato. Se il cookie non dispone di una scadenza, verrà eliminato quando il browser viene chiuso. Se è presente una scadenza, tuttavia, il cookie rimane memorizzato nel computer dell'utente fino alla data e intervallo di tempo specificato in scadenza. Quando un cookie viene eliminato definitivamente dal browser, non viene inviato al server web. Pertanto, la distruzione di un cookie è analoga all'utente la registrazione all'esterno del sito.

> [!NOTE]
> Naturalmente, un utente può rimuovere in modo proattivo i cookie archiviati nel proprio computer. In Internet Explorer 7, si potrebbe passare a strumenti, opzioni e fare clic sul pulsante Delete nella sezione Cronologia esplorazioni. Da qui, fare clic sul pulsante Elimina i cookie.


Il sistema di autenticazione form crea basato su sessione o la scadenza cookie a seconda del valore passato per il *persistCookie* parametro. Si ricordi che della classe FormsAuthentication GetAuthCookie SetAuthCookie e RedirectFromLoginPage accettano due parametri di input: *username* e *persistCookie*. Pagina di accesso creato nell'esercitazione precedente è incluso una memorizza dati controllo CheckBox, determinare se è stato creato un cookie permanente. Cookie permanenti sono basate su scadenza; i cookie non persistenti sono basati su sessione.

I concetti di timeout e slidingExpiration già descritti applicano lo stesso per entrambi i cookie basati su sessione e scadenza. È presente solo una differenza minore in esecuzione: quando si utilizza i cookie basato su scadenza con slidingTimeout impostato su true, la scadenza del cookie viene aggiornata solo quando più della metà del tempo specificato è trascorso.

Si aggiorna criteri timeout ticket di autenticazione del sito in modo che i ticket timeout dopo un'ora (60 minuti), utilizzando una scadenza. Per rendere effettiva questa modifica, aggiornare il file Web. config, aggiunta di un &lt;form&gt; elemento per il &lt;autenticazione&gt; elemento con il markup seguente:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Utilizzando un URL della pagina account di accesso diverso da Login. aspx

Poiché FormsAuthenticationModule reindirizza automaticamente gli utenti non autorizzati alla pagina di accesso, è necessario conoscere l'account di accesso URL della pagina. Questo URL è specificato dall'attributo loginUrl nel &lt;form&gt; elemento e valore predefinito è Login. aspx. Se si trasferisce su un sito Web esistente, è possibile che esiste già una pagina di accesso con un URL diverso, uno che è già stato corrispondente viene rimossa e indicizzato dai motori di ricerca. Anziché rinominare la pagina di accesso esistente in Login. aspx e rilievo collegamenti e segnalibri degli utenti, è possibile modificare invece l'attributo loginUrl affinché punti alla pagina di accesso.

Ad esempio, se la pagina di accesso è stata denominata SignIn.aspx e si trova nella directory degli utenti, si potrebbe indicare l'impostazione di configurazione loginUrl a ~/Users/SignIn.aspx come illustrato di seguito:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Poiché l'applicazione corrente già dispone di una pagina di accesso denominata Login. aspx, non è necessario specificare un valore personalizzato nel &lt;form&gt; elemento.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Passaggio 2: Utilizzo di ticket di autenticazione form senza cookie

Per impostazione predefinita il sistema di autenticazione form determina se memorizzare il ticket di autenticazione nella raccolta di cookie o incorporarle nell'URL basata sull'agente utente visita il sito. Tutti i browser desktop tradizionali come Internet Explorer, Firefox, Opera e Safari, i cookie di supporto, ma non tutti i dispositivi mobili.

I criteri di cookie usato dal sistema di autenticazione form dipende dall'impostazione cookieless nel &lt;form&gt; elemento che può essere assegnato uno dei quattro valori:

- UseCookies - specifica che i ticket di autenticazione basato su cookie verranno sempre utilizzati.
- UseUri - indica che i ticket di autenticazione basato su cookie non saranno mai utilizzati.
- Rilevamento automatico - se il profilo del dispositivo non supporta i cookie, ticket di autenticazione basato su cookie non vengono utilizzati; Se il profilo del dispositivo li supporta, viene utilizzato un meccanismo di probe per determinare se i cookie siano abilitati.
- UseDeviceProfile - all'impostazione predefinita. vengono utilizzati i ticket di autenticazione basata su cookie solo se il profilo del dispositivo li supporta. Non viene utilizzato alcun meccanismo di sondaggio.

Le impostazioni di rilevamento automatico e UseDeviceProfile si basano su un *profilo di dispositivo* nel determinare se utilizzare i ticket di autenticazione basato su cookie o senza cookie. ASP.NET gestisce un database di vari dispositivi e le relative funzionalità, ad esempio se supportano o meno i cookie, quale versione di JavaScript supportano e così via. Ogni volta che un dispositivo richiede una pagina web da un server web invia lungo una *agente utente* intestazione HTTP che identifica il tipo di dispositivo. ASP.NET associa automaticamente la stringa user-agent fornito al profilo corrispondente nel proprio database.

> [!NOTE]
> Questo database delle funzionalità del dispositivo viene archiviato in un numero di file XML che rispettano i [dello schema di File di definizione del Browser](https://msdn.microsoft.com/library/ms228122.aspx). I file di profilo di dispositivo predefiniti si trovano in % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. È anche possibile aggiungere file personalizzati all'App dell'applicazione\_cartella browser. Per altre informazioni, vedere [procedura: rilevare tipi di Browser in ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Poiché l'impostazione predefinita è UseDeviceProfile, ticket di autenticazione form senza cookie verrà usato quando si visita il sito da un dispositivo di cui il profilo segnala che non supporta i cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Codifica il Ticket di autenticazione nell'URL

I cookie sono un mezzo naturale per l'inclusione di informazioni dal browser in ciascuna richiesta per un determinato sito Web, motivo per cui le impostazioni di autenticazione form predefinito utilizzano cookie se il dispositivo visitando supportarle. Se non sono supportati i cookie, è necessario utilizzare un metodo alternativo per passare il ticket di autenticazione dal client al server. Una soluzione alternativa comune utilizzata in ambienti senza cookie è la codifica i dati del cookie nell'URL.

Il modo migliore per vedere come tali informazioni possono essere incorporate all'interno dell'URL consiste nel forzare il sito per l'utilizzo di ticket di autenticazione senza cookie. Questa operazione può essere eseguita impostando l'impostazione di configurazione senza cookie per UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Dopo aver apportato questa modifica, visitare il sito tramite un browser. Durante la visita come utente anonimo, gli URL avrà un aspetto esattamente come in precedenza. Ad esempio, durante la visita pagina default. aspx barra degli indirizzi del browser Mostra l'URL seguente:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Tuttavia, all'accesso, il ticket di autenticazione form è incorporato nell'URL. Ad esempio, dopo la visita la pagina di accesso e accedere come Sam, è possibile sto restituito alla pagina default. aspx, ma l'URL in questo caso è:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Il ticket di autenticazione form è incorporato nell'URL. La stringa (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) rappresenta le informazioni di ticket di autenticazione con codifica esadecimale e sono gli stessi dati che sono in genere archiviati all'interno di un cookie.

Affinché i ticket di autenticazione senza cookie funzionare, il sistema deve codificare tutti gli URL della pagina per includere i dati del ticket di autenticazione, in caso contrario, il ticket di autenticazione verranno perso quando l'utente fa clic su un collegamento. Fortunatamente, questa logica incorporamento viene eseguita automaticamente. Per illustrare questa funzionalità, aprire la pagina default. aspx e aggiungere un controllo collegamento ipertestuale, impostare le relative proprietà NavigateUrl e testo collegamento di Test e SomePage.aspx, rispettivamente. Non è importante che realmente non è una pagina nel progetto denominato SomePage.aspx.

Salvare le modifiche apportate a default. aspx e quindi vi accede tramite un browser. Accedere al sito in modo che il ticket di autenticazione form è incorporato nell'URL. Successivamente, default. aspx, fare clic sul collegamento collegamento di Test. Cosa è successo? Se è non presente alcuna pagina denominata SomePage.aspx, quindi si è verificato un errore 404, ma che non è importante qui. Al contrario, lo stato attivo nella barra degli indirizzi del browser. Si noti che include il ticket di autenticazione form nell'URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

SomePage.aspx l'URL nel collegamento è stato automaticamente convertito in un URL che è incluso il ticket di autenticazione - non è necessario scrivere codice. Il ticket di autenticazione form verrà automaticamente incorporato nell'URL per qualsiasi collegamento ipertestuale che non iniziano con `http://` o `/`. Non è importante se il collegamento ipertestuale viene visualizzato in una chiamata a Response. Redirect, in un controllo collegamento ipertestuale o in un elemento ancoraggio HTML (ad esempio, `<a href="...">...</a>`). Fino a quando l'URL non è analogo a quello `http://www.someserver.com/SomePage.aspx` o `/SomePage.aspx`, il ticket di autenticazione form verrà incorporato automaticamente.

> [!NOTE]
> Ticket di autenticazione form senza cookie rispettare gli stessi criteri di timeout dei ticket di autenticazione basata su cookie. Tuttavia, i ticket di autenticazione cookieless più inclini a attacchi di tipo replay poiché il ticket di autenticazione è incorporato direttamente nell'URL. Si supponga di un utente visita un sito Web, esegue l'accesso e quindi Incolla l'URL in un messaggio di posta elettronica a un collega. Se il collega fa clic sul collegamento prima che venga raggiunta la scadenza, questi verranno registrati come l'utente che ha inviato il messaggio di posta elettronica.


## <a name="step-3-securing-the-authentication-ticket"></a>Passaggio 3: Protezione il Ticket di autenticazione

Il ticket di autenticazione form viene trasmesso in un cookie o incorporato direttamente all'interno dell'URL. Oltre alle informazioni di identità, il ticket di autenticazione può includere anche dati utente (come verrà illustrato nel passaggio 4). Di conseguenza, è importante che i dati del ticket sono crittografati da intrusi e (anche ancora più importante) che il sistema di autenticazione form in grado di garantire che il ticket non è stato manomesso.

Per garantire la privacy dei dati del ticket, il sistema di autenticazione form possibile crittografare i dati del ticket. Crittografare i dati del ticket invia informazioni potenzialmente riservate nella rete in testo normale.

Per garantire l'autenticità del ticket, il sistema di autenticazione form deve *convalidare* il ticket. Convalida consiste nel garantire che una particolare parte di dati non è stata modificata e viene eseguita tramite un  *[il codice di autenticazione (MAC) del messaggio](http://en.wikipedia.org/wiki/Message_authentication_code)*. In pratica, il MAC è una piccola parte di informazioni che identificano i dati che devono essere convalidata (in questo caso, il ticket). Se vengono modificati i dati rappresentati da MAC, quindi il MAC e i dati non corrisponderanno. Inoltre, è difficile a livello di calcolo per un pirata informatico sia modificare i dati e generare il proprio MAC per la corrispondenza con i dati modificati.

Quando la creazione (o la modifica) un ticket, il sistema di autenticazione form crea un MAC e lo collega a dati del ticket. Quando arriva una richiesta successiva, il sistema di autenticazione form confronta i dati MAC e ticket per convalidare l'autenticità del dati del ticket. La figura 3 illustra il flusso di lavoro graficamente.


[![Viene verificato se autenticità del Ticket tramite un MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Figura 03**: viene verificato se autenticità del Ticket tramite un MAC ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Le misure di sicurezza vengono applicate al ticket di autenticazione dipende dall'impostazione di protezione nel &lt;form&gt; elemento. L'impostazione di protezione può essere assegnato a uno dei tre valori seguenti:

- All - il ticket è sia crittografato e firmato digitalmente (impostazione predefinita).
- Crittografia - solo la crittografia viene applicata, non viene generato alcun MAC.
- None: il ticket viene crittografato né firmato digitalmente.
- Convalida: un MAC viene generata, ma i dati del ticket viene inviati in rete in testo normale.

Microsoft consiglia vivamente utilizzando tutte le impostazioni.

### <a name="setting-the-validation-and-decryption-keys"></a>Impostazione di convalida e le chiavi di decrittografia

La crittografia e hash di algoritmi utilizzati dal sistema di autenticazione form per crittografare e convalidare il ticket di autenticazione sono personalizzabili tramite il [ &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx) in Web. config. Tabella 2 contorni il &lt;machineKey&gt; attributi dell'elemento e i relativi valori possibili.

| **Attributo** | **Descrizione** |
| --- | --- |
| decrittografia | Indica l'algoritmo utilizzato per la crittografia. Questo attributo può avere uno dei quattro valori seguenti: - Auto: il valore predefinito; Determina l'algoritmo in base alla lunghezza dell'attributo decryptionKey. -AES - utilizza il [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmo. -DES - utilizza il [Data Encryption Standard (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) questo algoritmo è considerato vulnerabile a livello di calcolo e non deve essere utilizzato. -3DES - utilizza il [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmo, che applica l'algoritmo DES tre volte. |
| decryptionKey | La chiave privata usata dall'algoritmo di crittografia. Questo valore deve essere una stringa esadecimale di lunghezza appropriata (in base al valore decrittografia), genera automaticamente o il valore aggiunto, IsolateApps. Aggiunta di IsolateApps indica ad ASP.NET di utilizzare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |
| convalida | Indica l'algoritmo utilizzato per la convalida. Questo attributo può avere uno dei quattro valori seguenti: - AES - Usa l'algoritmo Advanced Encryption Standard (AES). -MD5 - utilizza il [Message-Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmo. -SHA1 - utilizza il [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmo (impostazione predefinita). -3DES - Usa l'algoritmo Triple DES. |
| validationKey | La chiave privata usata dall'algoritmo di convalida. Questo valore deve essere una stringa esadecimale di lunghezza appropriata (in base al valore in fase di convalida), genera automaticamente o il valore aggiunto, IsolateApps. Aggiunta di IsolateApps indica ad ASP.NET di utilizzare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |

**Tabella 2**: il &lt;machineKey&gt; attributi elemento

Una trattazione completa di queste opzioni di crittografia e convalida e i professionisti e gli svantaggi dei vari algoritmi, esula dall'ambito di questa esercitazione. Per un'analisi approfondita esaminare questi problemi, incluse indicazioni sui quali algoritmi di crittografia e convalida da utilizzare, quali le lunghezze di chiave da usare e il modo migliore per la generazione di queste chiavi, fare riferimento a *Professional ASP.NET 2.0 sicurezza, l'appartenenza e gestione dei ruoli* .

Per impostazione predefinita, le chiavi usate per la crittografia e convalida vengono generate automaticamente per ogni applicazione e tali chiavi vengono archiviate in Autorità di sicurezza locale (LSA). In breve, le impostazioni predefinite garantiscono che le chiavi univoche di un server web dal server web e dell'applicazione per l'applicazione. Di conseguenza, questo comportamento predefinito non funzionerà per i due scenari seguenti:

- **Web farm** - in un [farm web](http://en.wikipedia.org/wiki/Web_farm) scenario, una singola applicazione web è ospitato in più server web ai fini della scalabilità e ridondanza. Ogni richiesta in ingresso viene inviato a un server nella farm, vale a dire che tutta la durata della sessione dell'utente, server diversi può essere utilizzato per gestire il suo varie richieste. Di conseguenza, ogni server deve usare le stesse chiavi di crittografia e convalida in modo che il ticket di autenticazione form creato, crittografato e convalidato in un server può essere decrittografato e convalidato in un altro server nella farm.
- **Tra l'applicazione di condivisione Ticket** -un singolo server web può ospitare più applicazioni ASP.NET. Se è necessario per queste applicazioni diverse condividere un ticket di autenticazione form singolo, è essenziale che le relative chiavi di crittografia e convalida corrispondano.

Quando si lavora in una web farm impostazione o condividere i ticket di autenticazione tra più applicazioni sullo stesso server, è necessario configurare il &lt;machineKey&gt; elemento nelle applicazioni interessate in modo che i relativi decryptionKey e i valori di validationKey individuare le corrispondenze.

Mentre nessuno degli scenari precedenti è valido per l'applicazione di esempio, è comunque possibile specificare decryptionKey esplicite e i valori di validationKey e definire gli algoritmi da utilizzare. Aggiungere un &lt;machineKey&gt; impostazione nel file Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Per ulteriori informazioni consultare [procedura: configurare MachineKey in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> I valori decryptionKey e validationKey sono stati ricavati dalla [Steve Gibson](http://www.grc.com/stevegibson.htm)del [pagina password perfetto](https://www.grc.com/passwords.htm), che genera 64 caratteri esadecimali casuali per visitare ogni pagina. Per ridurre la probabilità di queste chiavi effettua arrivare in applicazioni di produzione, si consiglia di sostituire le chiavi precedenti con quelle generate in modo casuale dalla pagina password perfetto.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Passaggio 4: Archiviazione dei dati utente aggiuntive nel Ticket

Molte applicazioni web su cui visualizzare informazioni o visualizzazione della pagina basata sull'utente attualmente connesso. Ad esempio, una pagina web potrebbe mostrare il nome dell'utente e la data che ha effettuato l'ultimo accesso nell'angolo superiore di ogni pagina. Il ticket di autenticazione form Memorizza nome utente dell'utente attualmente connesso, ma quando qualsiasi altra informazione è necessaria, la pagina deve essere eseguita all'archivio dell'utente, in genere un database - per cercare le informazioni non archiviate nel ticket di autenticazione.

Con un po' di codice che è possibile archiviare le informazioni utente aggiuntive nel ticket di autenticazione form. Tali dati possono essere espressi mediante il [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)del [UserData proprietà](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Si tratta di una soluzione ideale per contenere piccole quantità di informazioni relative all'utente che è in genere necessaria. Il valore specificato in UserData proprietà è incluso come parte del cookie di ticket di autenticazione e, come gli altri campi ticket, viene crittografata e convalidata in base alla configurazione del sistema di autenticazione form. Per impostazione predefinita, UserData è una stringa vuota.

Per archiviare i dati utente nel ticket di autenticazione, è necessario scrivere un po' di codice nella pagina di accesso che acquisisce le informazioni specifiche dell'utente e la archivia nel ticket. Poiché UserData è una proprietà di tipo stringa, i dati in essa archiviati devono essere serializzati in modo corretto sotto forma di stringa. Ad esempio, si supponga che l'archivio utente inclusi data di ciascun utente di nascita e il nome dell'azienda e Desideravamo archiviare questi due valori di proprietà nel ticket di autenticazione. È stato possibile serializzare questi valori in una stringa concatenando data dell'utente della stringa di nascita con una barra verticale (|), seguito dal nome del datore di lavoro. Per un utente nato 15 agosto 1974 che può essere usato per Northwind Traders, è necessario assegnare la proprietà UserData la stringa: 1974-08-15 | Northwind Traders.

Ogni volta che è necessario accedere ai dati archiviati nel ticket, possiamo scopo e la deserializzazione della proprietà UserData sul FormsAuthenticationTicket della richiesta corrente. Nel caso la data di nascita e datore di lavoro di esempio di nome, è necessario suddividere la stringa UserData in due sottostringhe in base al delimitatore (|).


[![Le informazioni utente aggiuntive possono essere archiviate nel Ticket di autenticazione](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Figura 04**: aggiuntive utente le informazioni possono essere archiviati nel Ticket di autenticazione ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>La scrittura di informazioni a UserData

Sfortunatamente, aggiunta di informazioni specifiche dell'utente a un ticket di autenticazione form non è così semplice come ci si aspetterebbe. La proprietà UserData della classe FormsAuthenticationTicket è di sola lettura e può essere specificata solo tramite il costruttore della classe FormsAuthenticationTicket. Quando si specifica la proprietà UserData nel costruttore, è anche necessario fornire il ticket's altri valori: il nome utente, la data di emissione, scadenza e così via. Quando abbiamo creato la pagina di accesso nell'esercitazione precedente, questa è stata tutti gestita automaticamente dalla classe FormsAuthentication. Quando si aggiungono UserData a FormsAuthenticationTicket, è necessario scrivere codice per replicare la maggior parte delle funzionalità già fornita dalla classe FormsAuthentication.

Esaminiamo il codice necessario per l'utilizzo di UserData aggiornando la pagina aspx per registrare informazioni aggiuntive relative all'utente per il ticket di autenticazione. Fingono che l'archivio utente contiene informazioni sulla società l'utente funziona per e al titolo, e che si desidera acquisire queste informazioni nel ticket di autenticazione. Aggiornamento LoginButton evento Click della pagina Login. aspx in modo che il codice simile al seguente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Esaminiamo questo codice una riga alla volta. Il metodo avvia definendo quattro matrici di stringhe: gli utenti, password, companyName e titleAtCompany. Queste matrici contengono i nomi utente, password, i nomi di società e titoli per gli account utente nel sistema, di cui sono disponibili tre: Scott Jisun e Sam. In un'applicazione reale, questi valori verrebbero sottoposti a query dall'archivio dell'utente, non a livello di codice nel codice sorgente della pagina.

Nell'esercitazione precedente, se le credenziali specificate non sono valide semplicemente telefonata FormsAuthentication (UserName.Text, RememberMe.Checked), che eseguiva i passaggi seguenti:

1. Creazione di form ticket di autenticazione
2. Scrittura del ticket in archivio appropriato. Per i ticket di autenticazione basata su cookie, viene utilizzato insieme i cookie del browser; per i ticket di autenticazione senza cookie, i dati del ticket sono serializzati nell'URL
3. Reindirizzato l'utente alla pagina appropriata

Questi passaggi vengono replicati nel codice precedente. In primo luogo, la stringa che verrà infine archiviato nella proprietà UserData è formata combinando il nome della società e il titolo, che delimitano i due valori con un carattere di barra verticale (|).

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

Successivamente, FormsAuthentication.GetAuthCookie metodo viene richiamato, che crea il ticket di autenticazione, crittografa la convalida in base alle impostazioni di configurazione e lo inserisce in un oggetto HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked);

Per poter funzionare con FormAuthenticationTicket incorporato all'interno del cookie, è necessario chiamare la classe FormAuthentication [decrittografare metodo](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), passando il valore del cookie.

Ticket FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value);

È quindi possibile creare una *nuovo* istanza FormsAuthenticationTicket in base ai valori del FormsAuthenticationTicket esistente. Tuttavia, questo nuovo ticket include le informazioni specifiche dell'utente (userDataString).

FormsAuthenticationTicket newTicket = nuovo FormsAuthenticationTicket (ticket. Versione, ticket. Nome del ticket. Data di rilascio, ticket. Scadenza, ticket. IsPersistent, userDataString);

È quindi crittografare (e convalidare) la nuova istanza FormsAuthenticationTicket chiamando il [crittografare metodo](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)e riportare i dati crittografati (e convalidati) in authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Infine, authCookie viene aggiunto alla raccolta Cookies e viene chiamato il metodo GetRedirectUrl per determinare la tabella appropriata per inviare all'utente.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Tutto il codice è necessaria perché la proprietà UserData è di sola lettura e la classe FormsAuthentication non fornisce alcun metodo per specificare le informazioni UserData nei metodi GetAuthCookie, SetAuthCookie o RedirectFromLoginPage.

> [!NOTE]
> Il codice che viene esaminato solo archivia informazioni specifiche dell'utente in un ticket di autenticazione basato su cookie. Le classi responsabile per la serializzazione il ticket di autenticazione form all'URL sono interne a .NET Framework. In breve, è possibile archiviare i dati utente in un ticket di autenticazione form senza cookie.


### <a name="accessing-the-userdata-information"></a>Accesso alle informazioni UserData

A questo punto il nome della società e titolo di ogni utente viene archiviato nella proprietà UserData del ticket di autenticazione form quando effettuano l'accesso. Queste informazioni sono accessibili dal ticket di autenticazione in qualsiasi pagina senza richiedere un trip all'archivio dell'utente. Per illustrare come queste informazioni possono essere recuperate dalla proprietà UserData, opportuno aggiornare default. aspx in modo che il messaggio di benvenuto include non solo il nome dell'utente, ma anche la società che lavorano per e al titolo.

Attualmente, default. aspx contiene un pannello AuthenticatedMessagePanel con un controllo etichetta denominato WelcomeBackMessage. In questo pannello è visualizzato solo per gli utenti autenticati. Aggiornare il codice nella pagina del default. aspx\_caricare il gestore di evento in modo che sia simile al seguente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Se Request.IsAuthenticated è true, il del WelcomeBackMessage testo viene innanzitutto impostata su Bentornato, *username*. Quindi, la proprietà di User. Identity è eseguire il cast a un oggetto FormsIdentity in modo che sia possibile accedere FormsAuthenticationTicket sottostante. Una volta ottenuto il FormsAuthenticationTicket, abbiamo deserializzare la proprietà UserData nel nome della società e il titolo. Questa operazione viene eseguita suddividendo la stringa sul carattere di barra verticale. Il nome della società e il titolo, quindi vengono visualizzati nell'etichetta WelcomeBackMessage.

Figura 5 mostra una schermata di questa visualizzazione in azione. Accedere come Scott Visualizza un messaggio di back-benvenuto che include Scott aziendale e il titolo.


[![Vengono visualizzati l'attualmente registrati dell'utente aziendale e il titolo](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Figura 05**: vengono visualizzati il attualmente registrati dell'utente aziendale e titolo ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Proprietà UserData del ticket di autenticazione viene utilizzata come una cache per l'archivio dell'utente. Come qualsiasi cache, è necessario che venga aggiornato quando i dati sottostanti vengono modificati. Ad esempio, se è presente una pagina web da cui gli utenti possono aggiornare il proprio profilo, i campi memorizzati nella cache nella proprietà UserData devono essere aggiornati in modo da riflettere le modifiche apportate dall'utente.


## <a name="step-5-using-a-custom-principal"></a>Passaggio 5: Usando un'entità personalizzata

Ogni richiesta in ingresso FormsAuthenticationModule tenta di autenticare l'utente. Se è presente un ticket di autenticazione non scadute, FormsAuthenticationModule assegna la proprietà HttpContext a un nuovo oggetto GenericPrincipal. Questo oggetto GenericPrincipal ha un'identità di tipo FormsIdentity, che include un riferimento a ticket di autenticazione dei form. La classe GenericPrincipal contiene bare funzionalità minime necessarie per una classe che implementa IPrincipal: è sufficiente una proprietà Identity e un metodo IsInRole.

L'oggetto principal ha due responsabilità: per indicare quali ruoli a cui appartiene l'utente e fornire le informazioni di identità. Questa operazione viene eseguita tramite IsInRole dell'interfaccia IPrincipal (*roleName*) metodo e proprietà Identity, rispettivamente. La classe GenericPrincipal consente una matrice di stringhe di nomi di ruoli per essere specificato tramite il relativo costruttore; relativo IsInRole (*roleName*) metodo semplicemente controlla se il valore passato in *roleName* esiste all'interno della matrice di stringa. Quando FormsAuthenticationModule crea GenericPrincipal, passa una matrice di stringa vuota al costruttore del GenericPrincipal. Di conseguenza, qualsiasi chiamata a IsInRole restituirà sempre false.

La classe GenericPrincipal soddisfi le esigenze per la maggior parte degli scenari di autenticazione basata su form in cui non vengono utilizzati i ruoli. Per i casi in cui non è sufficiente la gestione di ruolo predefinita o quando è necessario associare un oggetto IIdentity personalizzato all'utente, è possibile creare un oggetto IPrincipal personalizzato durante il flusso di lavoro di autenticazione e assegnarlo alla proprietà HttpContext.

> [!NOTE]
> Come è possibile osservare in futuro le esercitazioni, quando ASP. Del ruoli .NET framework è abilitata viene creato un oggetto personalizzato dell'entità di tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) e sovrascrive l'oggetto GenericPrincipal creati dall'autenticazione form. Ciò avviene per personalizzare il metodo IsInRole del server principale all'interfaccia con l'API del framework ruoli.


Poiché è stato non in questione viene con ruoli ancora, l'unico motivo per cui che abbiamo per la creazione di un'entità personalizzata a questo punto, è possibile associare un oggetto IIdentity personalizzato per l'entità. Nel passaggio 4 è stato esaminato l'archiviazione delle informazioni utente aggiuntive nella proprietà UserData del ticket di autenticazione, in particolare il nome dell'utente aziendale e al titolo. Tuttavia, le informazioni UserData sono solo accessibili tramite il ticket di autenticazione e quindi solo come una stringa serializzata, vale a dire che ogni volta che si desidera visualizzare le informazioni utente archiviate nel ticket è necessario analizzare la proprietà UserData.

Creando una classe che implementa IIdentity e include proprietà CompanyName e titolo migliorare l'esperienza dello sviluppatore. In questo modo, uno sviluppatore possa accedere nome della società dell'utente attualmente connesso e titolo direttamente tramite le proprietà CompanyName e titolo senza sapere analizzare la proprietà UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Creazione dell'identità personalizzato e classi principali

Per questa esercitazione, creare gli oggetti principal e identity personalizzati nell'App\_cartella del codice. Per iniziare, aggiungere un'App\_cartella nel progetto del codice - pulsante destro del mouse sul nome del progetto in Esplora soluzioni, selezionare l'opzione Aggiungi cartella ASP.NET e scegliere App\_codice. L'App\_cartella del codice è una cartella speciale di ASP.NET che contiene file di classe specifico per il sito Web.

> [!NOTE]
> L'App\_cartella del codice deve essere utilizzata solo quando la gestione del progetto tramite il modello di progetto di sito Web. Se si utilizza il [modello di progetto applicazione Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), creare una cartella standard e aggiungere le classi che. Ad esempio, è possibile aggiungere una nuova cartella denominata classi e posizionarvi il codice.


Successivamente, aggiungere due nuovi file di classe per l'App\_cartella del codice, CustomIdentity.cs denominata uno e uno denominato CustomPrincipal.cs.


[![Aggiungere tutte le classi CustomPrincipal CustomIdentity al progetto](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Figura 06**: aggiungere tutte le classi CustomPrincipal CustomIdentity a un progetto ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


La classe CustomIdentity è responsabile dell'implementazione dell'interfaccia IIdentity, che definisce le proprietà AuthenticationType, IsAuthenticated e il nome. Oltre a queste proprietà richieste, si intende esporre il sottostante ticket di autenticazione form, nonché le proprietà per nome della società e il titolo dell'utente. Immettere il codice seguente nella classe CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Si noti che la classe include una variabile membro FormsAuthenticationTicket (\_ticket) e che queste informazioni ticket devono essere fornite mediante il costruttore. Questi dati ticket viene utilizzati nel restituire il nome dell'identità. la proprietà UserData viene analizzata per restituire i valori per le proprietà CompanyName e il titolo.

Successivamente, creare la classe CustomPrincipal. Poiché non presentano problemi con i ruoli a questo punto, il costruttore della classe CustomPrincipal accetta solo un oggetto CustomIdentity; il metodo IsInRole restituisce sempre false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Assegnazione di un oggetto CustomPrincipal al contesto di sicurezza della richiesta in ingresso

È ora disponibile una classe che estende la specifica di identità predefinito per includere proprietà CompanyName e Title, nonché una classe di entità personalizzata che utilizza l'identità personalizzata. Si è pronti per passaggio nella pipeline ASP.NET e assegna l'oggetto dell'entità personalizzata al contesto di sicurezza della richiesta in ingresso.

La pipeline ASP.NET accetta una richiesta in ingresso e lo elabora mediante una serie di passaggi. A ogni passaggio, viene generato un evento specifico, consentendo agli sviluppatori di toccare nella pipeline ASP.NET e modificare la richiesta in determinati momenti nel ciclo di vita. FormsAuthenticationModule, ad esempio, attende per ASP.NET generare il [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), a quel punto analizza la richiesta in ingresso per un ticket di autenticazione. Se viene trovato un ticket di autenticazione, un oggetto GenericPrincipal verrà creato e assegnato alla proprietà HttpContext.

Dopo l'evento, AuthenticateRequest della pipeline ASP.NET genera il [evento PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), da cui è in cui è possibile sostituire gli oggetti GenericPrincipal creati dalle FormsAuthenticationModule con un'istanza di nostri Oggetto CustomPrincipal. Figura 7 illustra il flusso di lavoro.


[![GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Figura 07**: il GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Per eseguire codice in risposta a un evento di pipeline ASP.NET, è possibile creare il gestore eventi appropriato in Global. asax oppure creare un modulo HTTP. Per questa esercitazione verrà creato il gestore dell'evento in Global. asax. Per iniziare, aggiungere Global. asax per il sito Web. Fare clic sul nome del progetto in Esplora soluzioni e aggiungere un elemento di tipo classe di applicazione globale denominato Global. asax.


[![Aggiungere un File Global. asax per il sito Web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Figura 08**: aggiungere un File Global. asax per il sito Web ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


Il modello di Global. asax predefinito include i gestori eventi per un numero di eventi della pipeline ASP.NET, tra cui inizio, fine e [evento di errore](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), tra gli altri. È possibile rimuovere questi gestori eventi, come ci non sono necessari per questa applicazione. L'evento che si è interessati è PostAuthenticateRequest. Aggiornare il file Global. asax in modo relativo tag è simile al seguente:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

L'applicazione\_OnPostAuthenticateRequest metodo viene eseguito ogni volta che il runtime di ASP.NET genera l'evento PostAuthenticateRequest, situazione che si verifica una volta in ogni richiesta di pagina in ingresso. Il gestore dell'evento inizia con un controllo per verificare se l'utente viene autenticato ed è stato autenticato tramite l'autenticazione basata su form. In tal caso, un nuovo oggetto CustomIdentity viene creato e passato ticket di autenticazione della richiesta corrente nel relativo costruttore. Successivamente, un oggetto CustomPrincipal è creato e passato l'oggetto di CustomIdentity appena creato nel relativo costruttore. Infine, il contesto di sicurezza della richiesta corrente viene assegnato all'oggetto CustomPrincipal appena creato.

Si noti che l'ultimo passaggio - associare l'oggetto CustomPrincipal il contesto di sicurezza della richiesta - assegna l'entità a due proprietà: HttpContext e thread. CurrentPrincipal. Le assegnazioni di questi due sono necessarie a causa della modalità contesti di sicurezza vengono gestiti in ASP.NET. Un contesto di sicurezza di .NET Framework associa ogni thread in esecuzione. Queste informazioni sono disponibili come un oggetto di IPrincipal attraverso il [oggetto Thread](https://msdn.microsoft.com/library/system.threading.thread.aspx)del [proprietà CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Che cos'è un chiaro è ASP.NET con le proprie informazioni di contesto di sicurezza (HttpContext).

In alcuni scenari, la proprietà di thread. CurrentPrincipal viene esaminata per determinare il contesto di sicurezza. in altri scenari, viene utilizzato HttpContext. Ad esempio, sono disponibili funzionalità di sicurezza in .NET che consentono agli sviluppatori di dichiarare in modo dichiarativo quali utenti o ruoli è possono creare un'istanza di una classe o richiamare metodi specifici (vedere [aggiungendo le regole di autorizzazione Business e l'utilizzo di livelli di dati PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Nel sistema il queste tecniche dichiarative determinano il contesto di sicurezza tramite la proprietà di thread. CurrentPrincipal.

In altri scenari, la proprietà HttpContext viene utilizzata. Ad esempio, nell'esercitazione precedente utilizzassimo questa proprietà per visualizzare il nome utente dell'utente attualmente connesso. Chiaramente, quindi, è fondamentale che le informazioni sul contesto di sicurezza nelle proprietà del thread. CurrentPrincipal e HttpContext corrispondano ai.

Il runtime di ASP.NET viene sincronizzato automaticamente questi valori di proprietà per noi. Tuttavia, la sincronizzazione si verifica dopo l'evento AuthenticateRequest, ma *prima di* l'evento PostAuthenticateRequest. Di conseguenza, quando si aggiunge un'entità personalizzata nell'evento PostAuthenticateRequest è necessario assicurarsi di assegnare manualmente il thread. CurrentPrincipal altrimenti CurrentPrincipal e HttpContext sarà sincronizzato. Vedere [Context vs. Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) per informazioni dettagliate su questo problema.

### <a name="accessing-the-companyname-and-title-properties"></a>L'accesso al CompanyName e proprietà titolo

Ogni volta che una richiesta arriva e viene inviata al motore di ASP.NET, l'applicazione\_genererà gestore dell'evento OnPostAuthenticateRequest in Global. asax. Se la richiesta è stata autenticata da FormsAuthenticationModule, il gestore dell'evento creerà un nuovo oggetto CustomPrincipal con un oggetto CustomIdentity basato su ticket di autenticazione dei form. Con questa logica posto, l'accesso alle informazioni sul nome della società e il titolo dell'utente attualmente connesso è estremamente semplice.

Tornare alla pagina\_gestore di eventi di caricamento in default. aspx, in cui nel passaggio 4 è scritto codice per recuperare il ticket di autenticazione form e analizzare la proprietà UserData per visualizzare il nome di società e il titolo dell'utente. Con gli oggetti CustomPrincipal e CustomIdentity in uso a questo punto, non è necessario analizzare i valori dalla proprietà UserData del ticket. Al contrario, è sufficiente ottenere un riferimento all'oggetto CustomIdentity e utilizzarne le CompanyName e titolo:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Riepilogo

In questa esercitazione abbiamo esaminato come personalizzare le impostazioni del sistema di autenticazione form tramite Web. config. È stato esaminato la modalità di gestione di scadenza del ticket di autenticazione e come le misure di crittografia e convalida sicurezza vengono usati per proteggere il ticket da analisi e la modifica. Infine, abbiamo parlato utilizzando proprietà UserData del ticket di autenticazione per archiviare informazioni utente aggiuntive nel ticket di se stesso e come utilizzare gli oggetti principal e identity personalizzati per esporre queste informazioni in modo più semplice.

Questa esercitazione si conclude l'esame dell'autenticazione basata su form in ASP.NET. L'esercitazione successiva avvia nostro viaggio nel framework di appartenenza.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Sezionando autenticazione basata su form](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Spiegazione: Autenticazione basata su form in ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Procedura: Proteggere autenticazione basata su form in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 sicurezza, l'appartenenza e gestione dei ruoli](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (codice ISBN: 978-0-7645-9698-8)
- [Protezione dei controlli di accesso](https://msdn.microsoft.com/library/ms178346.aspx)
- [Il &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Il &lt;form&gt; elemento per &lt;autenticazione&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Il &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [La comprensione dei Ticket di autenticazione form e Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video di formazione su argomenti contenuti in questa esercitazione

- [Come modificare le proprietà di autenticazione form](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Come eseguire l'autenticazione senza Cookie di configurazione e l'uso in un'applicazione ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Rilocazione dell'accesso ai moduli ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configurazione della chiave personalizzata di accesso ai moduli](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Aggiungere dati personalizzati al metodo di autenticazione](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usare oggetti Principal personalizzati](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft fin dal 1998. Scott funziona come un consulente, trainer e writer. È il suo ultimo libro  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore per questa esercitazione è stata Alicja Maziarz. Se si è interessati my articoli MSDN a futuri? In tal caso, drop me una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Precedente](an-overview-of-forms-authentication-cs.md)
> [Successivo](security-basics-and-asp-net-support-vb.md)
