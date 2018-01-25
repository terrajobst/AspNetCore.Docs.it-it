---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: I moduli di configurazione dell'autenticazione e argomenti avanzati (VB) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verrà esaminare le varie impostazioni di autenticazione form e come modificarli tramite l'elemento di form. Ciò comporterà un dettagliate..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: fe4c421f248e325b69be7cad6c10bcbedf59ae5f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Configurazione dell'autenticazione form e argomenti avanzati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> In questa esercitazione verrà esaminare le varie impostazioni di autenticazione form e come modificarli tramite l'elemento di form. Ciò comporterà un'analisi approfondita sul valore di timeout del ticket di autenticazione form, utilizzando una pagina di accesso con un URL personalizzato (ad esempio SignIn.aspx anziché Login.aspx) e il ticket di autenticazione form senza cookie di personalizzazione.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](an-overview-of-forms-authentication-vb.md) abbiamo esaminato i passaggi necessari per implementare l'autenticazione basata su form in un'applicazione ASP.NET, specificare le impostazioni di configurazione in Web. config per la creazione di un log nella pagina per la visualizzazione di diversi contenuto per gli utenti anonimi e autenticati. Tenere presente che è stato configurato il sito Web per utilizzare l'autenticazione basata su form impostando l'attributo mode del &lt;autenticazione&gt; elemento a un form. Il &lt;autenticazione&gt; elemento può includere facoltativamente un &lt;form&gt; elemento figlio, tramite il quale è possibile specificare un'ampia gamma di impostazioni di autenticazione form.

In questa esercitazione verrà esaminare le varie impostazioni di autenticazione form e come modificarli tramite il &lt;form&gt; elemento. Ciò comporterà un'analisi approfondita sul valore di timeout del ticket di autenticazione form, utilizzando una pagina di accesso con un URL personalizzato (ad esempio SignIn.aspx anziché Login.aspx) e il ticket di autenticazione form senza cookie di personalizzazione. Verrà inoltre esaminare più da vicino la composizione del ticket di autenticazione form e vedere le precauzioni che ASP.NET utilizza per garantire la sicurezza di ispezione e manomissione dei dati del ticket. Infine, esamineremo come archiviare dati aggiuntivi dell'utente nel ticket di autenticazione form e come modellare i dati tramite un oggetto entità personalizzato.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Passaggio 1: Esame di &lt;form&gt; le impostazioni di configurazione

Il sistema di autenticazione form in ASP.NET offre una serie di impostazioni di configurazione che possono essere personalizzati in modo da applicazioni. Sono incluse le impostazioni come: la durata dell'autenticazione basata su form ticket; il tipo di protezione viene applicato il ticket; in quali autenticazione cookieless condizioni vengono utilizzati il ticket; il percorso per la pagina di accesso. e altre informazioni. Per modificare i valori predefiniti, aggiungere un [ &lt;form&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) come elemento figlio di [ &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx), che specifica le proprietà i valori che si desidera personalizzare come attributi XML nel modo seguente:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

Tabella 1 sono riepilogate le proprietà che possono essere personalizzate tramite la &lt;form&gt; elemento. Poiché il file Web. config è un file XML, i nomi di attributo nella colonna sinistra maiuscole e minuscole.

| **Attributo** | **Descrizione** |
| --- | --- |
| cookieless | Questo attributo specifica in quali condizioni è memorizzato il ticket di autenticazione in un cookie e viene incorporato nell'URL. Valori consentiti sono: UseCookies; UseUri; Rilevamento automatico; e UseDeviceProfile (impostazione predefinita). Passaggio 2 esamina questa impostazione in modo più dettagliato. |
| defaultUrl | Indica l'URL che gli utenti vengono reindirizzati a dopo l'accesso dalla pagina di accesso se è presente alcun valore RedirectUrl specificato nella stringa di query. Il valore predefinito è default. aspx. |
| dominio | Quando si utilizza il ticket di autenticazione basato su cookie, questa impostazione specifica il valore di dominio del cookie s. Il valore predefinito è una stringa vuota, provocando il browser da usare il dominio da cui è stato rilasciato (ad esempio www.yourdomain.com). In questo caso, il cookie verrà **non** da inviare quando apportato richieste per i sottodomini, ad esempio admin.yourdomain.com. Se si desidera che il cookie deve essere passato a tutti i sottodomini è necessario personalizzare l'attributo di dominio se è impostato su <sottodominio>.nomedominio.com. |
| enableCrossAppRedirects | Valore booleano che indica se gli utenti autenticati sono memorizzati quando reindirizzato all'URL in altre applicazioni web nello stesso server. Il valore predefinito è false. |
| loginUrl | L'URL della pagina di accesso. Il valore predefinito è login.aspx. |
| name | Quando si utilizza il ticket di autenticazione basato su cookie, il nome del cookie. Il valore predefinito è. ASPXAUTH. |
| path | Quando si utilizza il ticket di autenticazione basato su cookie, questa impostazione specifica l'attributo di percorso del cookie s. L'attributo path consente a uno sviluppatore limitare l'ambito di un cookie a una gerarchia di directory specifico. Il valore predefinito è /, che informa il browser invii il cookie di ticket di autenticazione a qualsiasi richiesta eseguita al dominio. |
| protezione | Indica le tecniche utilizzate per proteggere il ticket di autenticazione form. I valori consentiti sono: tutte (predefinito); Crittografia; None; e la convalida. Queste impostazioni sono illustrate in dettaglio nel passaggio 3. |
| requireSSL | Valore booleano che indica se è necessaria una connessione SSL per trasmettere il cookie di autenticazione. Il valore predefinito è false. |
| slidingExpiration | Valore booleano che indica che se il timeout del cookie s di autenticazione viene reimpostato ogni volta che l'utente visita il sito durante una singola sessione. Il valore predefinito è true. I criteri di timeout ticket di autenticazione verrà discusso più dettagliatamente nella specifica sezione di valore di Timeout Ticket. |
| timeout | Specifica il tempo, espresso in minuti, trascorso il quale il cookie di ticket di autenticazione scade. Il valore predefinito è 30. I criteri di timeout ticket di autenticazione verrà discusso più dettagliatamente nella specifica sezione di valore di Timeout Ticket. |

**Tabella 1**: un riepilogo di &lt;form&gt; attributi dell'elemento

In ASP.NET 2.0 e versioni successive, l'impostazione predefinita i valori di autenticazione form sono hardcoded nella classe FormsAuthenticationConfiguration in .NET Framework. Su base dall'applicazione nel file Web. config, è necessario applicare le modifiche. Questo comportamento è diverso da ASP.NET 1. x, in cui i valori di autenticazione moduli predefiniti sono stati archiviati nel file Machine. config (e pertanto può essere modificati mediante la modifica di Machine. config). Mentre per l'argomento di ASP.NET 1. x, è opportuno ricordare che un numero di impostazioni di sistema di autenticazione form avere valori predefiniti diversi in ASP.NET 2.0 e oltre a ASP.NET 1. x. Se si esegue la migrazione dell'applicazione da un ambiente di 1. x ASP.NET, è importante essere a conoscenza di queste differenze. Consultare [il &lt;form&gt; documentazione tecnica elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) per un elenco delle differenze.

> [!NOTE]
> Diverse impostazioni di autenticazione form, ad esempio il timeout, dominio e il percorso, specificano i dettagli per il cookie di ticket di autenticazione form risultante. Per altre informazioni su cookie, il funzionamento e le varie proprietà, vedere [questa esercitazione i cookie](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Specificando il valore di Timeout del Ticket.

Il ticket di autenticazione form è un token che rappresenta un'identità. Con i ticket di autenticazione basato su cookie, questo token è mantenuto in formato di un cookie e inviato al server web per ogni richiesta. Il possesso del token, in pratica, dichiara e faccio *username*, si hanno già effettuato l'accesso e viene utilizzato in modo che l'identità dell'utente può essere memorizzato in occasione di pagina.

Il ticket di autenticazione form non include solo l'identità dell'utente, ma contiene anche informazioni per garantire l'integrità e la protezione del token. Dopo tutto, non si nefandi utente non deve essere in grado di creare un token contraffatto o per modificare un token legalmente in qualche modo underhanded.

Un tale bit di informazioni incluse nel ticket è un *scadenza*, ovvero la data e ora il ticket non è più valido. Ogni volta che FormsAuthenticationModule controlla un ticket di autenticazione, assicura che la scadenza del ticket non ha ancora superato. In caso affermativo, ignora il ticket e identifica l'utente come anonimo. Questa misura consente di proteggere da attacchi di riproduzione. Senza una scadenza, se un pirata informatico è stato in grado di ottenere proprio dimestichezza ticket di autenticazione valido di un utente, ad esempio accedere fisicamente al computer e rooting tramite i cookie, potrebbe inviare una richiesta al server con il ticket di autenticazione e ottenere l'accesso. Mentre la scadenza non impedisce di questo scenario, non limita la finestra durante i quali un attacco può avere esito positivo.

> [!NOTE]
> Passaggio 3 informazioni tecniche aggiuntive utilizzate dal sistema di autenticazione form per proteggere il ticket di autenticazione.


Quando si crea il ticket di autenticazione, il sistema di autenticazione form determina la scadenza del consultando l'impostazione di timeout. Come indicato nella tabella 1, il timeout di definizione delle impostazioni predefinite per 30 minuti, vale a dire che, quando viene creato il ticket di autenticazione form di scadenza è impostata su una data e ora, 30 minuti in futuro.

La scadenza definisce un'ora assoluta in futuro in cui scade il ticket di autenticazione dei form. Ma in genere gli sviluppatori di implementare una scadenza variabile, che viene reimpostato ogni volta che l'utente effettua nuovamente richieste del sito. Questo comportamento è determinato dalle impostazioni slidingExpiration. Se impostato su true (impostazione predefinita), ogni volta che un utente viene autenticato FormsAuthenticationModule, aggiorna la scadenza del ticket. Se impostato su false, la scadenza non viene aggiornata a ogni richiesta, causando il ticket scadenza esattamente timeout numero di minuti passati quando il ticket è stato inizialmente creato.

> [!NOTE]
> La scadenza archiviata nel ticket di autenticazione è una data assoluta e il valore di ora, ad esempio 2 agosto 2008 11:34 AM. Inoltre, la data e ora sono rispetto all'ora locale del server web. Questa decisione progettuale, può avere alcuni effetti collaterali interessanti intorno all'ora legale (DST), ovvero quando orologi negli Stati Uniti vengono spostati Avanti (presupponendo che il server web è ospitato in una lingua in cui viene osservata l'ora legale) di un'ora. Si consideri cosa accadrebbe per un sito Web ASP.NET con una scadenza 30 minuti quasi l'ora di inizio dell'ora legale (ovvero alle 2:00). Si supponga che un visitatore accede al sito 11 marzo 2008 alle ore 1:55. Questo genera un ticket di autenticazione form che scade l'11 marzo 2008 alle ore 2:25 (in futuro per 30 minuti). Tuttavia, quando 2:00 AM RTM, l'orologio passa alle 3:00 a causa dell'ora legale. Quando l'utente carica una nuova pagina 6 minuti dopo l'accesso (alle 3:01:00), FormsAuthenticationModule viene rilevato che il ticket è scaduto e reindirizza l'utente alla pagina di accesso. Per una discussione più approfondita su questo e altri singolarità che timeout ticket di autenticazione, nonché soluzioni alternative, selezionare una copia di Stefan Schackow *Professional ASP.NET 2.0 sicurezza e l'appartenenza al ruolo Gestione* (ISBN: 978-0-7645-9698-8).


La figura 1 illustra il flusso di lavoro quando la proprietà slidingExpiration viene impostata su false e di timeout è impostato su 30. Si noti che il ticket di autenticazione generato al momento dell'accesso contiene la data di scadenza e questo valore non viene aggiornato nelle richieste successive. Se FormsAuthenticationModule rileva che il ticket è scaduto, lo ignora e lo considera la richiesta anonima.


[![Una rappresentazione grafica di slidingExpiration di scadenza quando del Ticket di autenticazione dei form è false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Figura 01**: una rappresentazione grafica di slidingExpiration di scadenza quando del Ticket di autenticazione dei form è false ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


La figura 2 mostra il flusso di lavoro quando slidingExpiration viene impostata su true e il timeout è impostato su 30. Quando viene ricevuta una richiesta autenticata (con un ticket non scadute) viene aggiornata la scadenza del timeout numero di minuti in futuro.


[![Una rappresentazione grafica dei Ticket di autenticazione form quando slidingExpiration è true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Figura 02**: una rappresentazione grafica del Ticket di autenticazione form quando slidingExpiration è true ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


Quando si utilizza il ticket di autenticazione basato su cookie (impostazione predefinita), questa discussione diventa più complicata, poiché i cookie possono essere anche i propri oggetti scaduti nella specificato. Scadenza di un cookie (o assenza) indica al browser quando il cookie deve essere eliminato. Se il cookie non dispone di una scadenza, verrà eliminato quando il browser viene chiuso. Se è presente una scadenza, tuttavia, il cookie rimane memorizzato nel computer dell'utente fino alla data e intervallo di tempo specificato in scadenza. Quando un cookie viene eliminato definitivamente dal browser, non viene inviato al server web. Pertanto, l'eliminazione di un cookie è analoga all'utente dal sito di registrazione.

> [!NOTE]
> Naturalmente, un utente in modo proattivo può rimuovere i cookie archiviati nel proprio computer. In Internet Explorer 7, è necessario passare a strumenti, opzioni e fare clic sul pulsante di eliminazione nella sezione Cronologia esplorazioni. Da qui, fare clic sul pulsante Elimina i cookie.


Il sistema di autenticazione form basato su sessione o scadenza cookie a seconda del valore passato al vengono creati il *persistCookie* parametro. Tenere presente che i metodi della classe FormsAuthentication GetAuthCookie SetAuthCookie e RedirectFromLoginPage accettano due parametri di input: *username* e *persistCookie*. Pagina di accesso che è stati creati nell'esercitazione precedente è incluso una memorizza dati controllo CheckBox, determinare se è stato creato un cookie permanente. I cookie permanenti sono basate su scadenza; i cookie non permanenti sono basati su sessione.

I concetti di timeout e slidingExpiration già illustrati applicano a entrambi i cookie basato sulla sessione e scadenza. È solo una differenza minore in esecuzione: quando si utilizza i cookie basato su scadenza con slidingTimeout impostato su true, la scadenza del cookie viene aggiornata solo quando più della metà del tempo specificato è trascorso.

Si aggiorna criteri timeout ticket di autenticazione del sito in modo che i ticket di timeout dopo un'ora (60 minuti), utilizzando una scadenza. Per rendere effettiva questa modifica, aggiornare il file Web. config, aggiunta di un &lt;form&gt; elemento per il &lt;autenticazione&gt; elemento con il markup seguente:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Utilizzando un URL di pagina di accesso diverso da Login

Poiché FormsAuthenticationModule reindirizza automaticamente gli utenti non autorizzati alla pagina di accesso, è necessario conoscere l'URL della pagina di accesso. Questo URL è specificato dall'attributo loginUrl nel &lt;form&gt; elemento e valore predefinito è login.aspx. Se si esegue il porting su un sito Web esistente, potrebbe essere già una pagina di accesso con un URL diverso, che è già stata rimossa e indicizzato dai motori di ricerca. Anziché ridenominare la pagina di accesso esistente Login e interrompere i collegamenti e segnalibri degli utenti, è invece possibile modificare l'attributo loginUrl per puntare alla pagina di accesso.

Se la pagina di accesso è stata denominata SignIn.aspx e si trova nella directory degli utenti, ad esempio, si potrebbe indicare l'impostazione di configurazione loginUrl per ~/Users/SignIn.aspx come illustrato di seguito:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Poiché l'applicazione corrente già dispone di una pagina di accesso login, non è necessario specificare un valore personalizzato nel &lt;form&gt; elemento.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Passaggio 2: Utilizzo di ticket di autenticazione form senza cookie

Per impostazione predefinita il sistema di autenticazione form determina se memorizzare il ticket di autenticazione nella raccolta di cookie o incorporarli nell'URL in base all'agente utente visita il sito. Tutti i browser desktop tradizionali come i cookie di Internet Explorer, Firefox, Opera e Safari, supporto, ma non tutti i dispositivi portatili.

I criteri di cookie usato dal sistema di autenticazione form dipende dall'impostazione cookieless nel &lt;form&gt; elemento che può essere assegnato uno dei quattro valori:

- UseCookies - specifica che verranno utilizzati sempre i ticket di autenticazione basato su cookie.
- UseUri - indica che i ticket di autenticazione basato su cookie non verranno mai utilizzati.
- Rilevamento automatico, se il profilo del dispositivo non supporta i cookie, ticket di autenticazione basato su cookie non vengono utilizzati. Se il profilo del dispositivo li supporta, viene utilizzato un meccanismo di probe per determinare se i cookie siano abilitati.
- UseDeviceProfile - impostazione predefinita. utilizza il ticket di autenticazione basato su cookie solo se il profilo del dispositivo li supporta. Non viene utilizzato alcun meccanismo di sondaggio.

Le impostazioni di rilevamento automatico e UseDeviceProfile si basano su un *profilo di dispositivo* nel determinare se utilizzare i ticket di autenticazione basato su cookie o senza cookie. ASP.NET mantiene un database di vari dispositivi e le relative funzionalità, ad esempio se supportano i cookie, la versione di JavaScript supportano e così via. Ogni volta che un dispositivo richiede una pagina web da un server web invia lungo un *agente utente* intestazione HTTP che identifica il tipo di dispositivo. ASP.NET associa automaticamente la stringa agente utente fornito al profilo corrispondente nel proprio database.

> [!NOTE]
> Questo database delle funzionalità del dispositivo viene archiviato in un numero di file XML che soddisfano i requisiti di [dello schema di File di definizione del Browser](https://msdn.microsoft.com/library/ms228122.aspx). I file di profilo di dispositivo predefiniti si trovano in % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. È anche possibile aggiungere file personalizzati dell'applicazione App\_cartella browser. Per ulteriori informazioni, vedere [procedura: rilevare tipi di Browser in ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Poiché l'impostazione predefinita è UseDeviceProfile, ticket di autenticazione form senza cookie da utilizzare quando si visita il sito da un dispositivo di cui il profilo segnala che non supporta i cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Codifica il Ticket di autenticazione nell'URL

I cookie sono un mezzo naturale per l'inclusione di informazioni dal browser in ciascuna richiesta per un determinato sito Web, motivo per cui le impostazioni di autenticazione form predefinito utilizzano i cookie, se il controllo dispositivo li supporta. Se non sono supportati i cookie, è necessario utilizzare un metodo alternativo per passare il ticket di autenticazione dal client al server. Una soluzione alternativa comune utilizzata in ambienti senza cookie è per codificare i dati del cookie nell'URL.

Il modo migliore per vedere come tali informazioni possono essere incorporate all'interno dell'URL consiste nel forzare il sito per l'utilizzo di ticket di autenticazione senza cookie. Questa operazione può essere eseguita impostando l'impostazione di configurazione senza cookie per UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Dopo aver apportato questa modifica, visitare il sito tramite un browser. Durante la visita come utente anonimo, gli URL avrà un aspetto esattamente come in precedenza. Ad esempio, quando si visita la pagina Default.aspx barra degli indirizzi del browser Mostra l'URL seguente:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Tuttavia, all'accesso, il ticket di autenticazione form è incorporato nell'URL. Dopo la visita la pagina di accesso e accedere come Sam, ad esempio, sono restituito alla pagina Default.aspx, ma l'URL in questo caso è:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Il ticket di autenticazione form è incorporato nell'URL. La stringa (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) rappresenta le informazioni di ticket di autenticazione con codifica esadecimale e sono gli stessi dati che sono in genere archiviati all'interno di un cookie.

Affinché i ticket di autenticazione senza cookie funzionare, il sistema deve codificare tutti gli URL della pagina per includere i dati del ticket di autenticazione, in caso contrario il ticket di autenticazione verranno persi quando l'utente fa clic su un collegamento. Fortunatamente, la logica di incorporamento viene eseguita automaticamente. Per illustrare questa funzionalità, aprire la pagina Default.aspx e aggiungere un controllo collegamento ipertestuale, impostazione delle relative proprietà NavigateUrl e testo collegamento di Test e SomePage.aspx, rispettivamente. Non è importante che realmente non è una pagina nel progetto denominato SomePage.aspx.

Salvare le modifiche a Default.aspx e quindi vi accede tramite un browser. Accedere al sito in modo che il ticket di autenticazione form è incorporato nell'URL. Successivamente, da Default.aspx, fare clic sul collegamento di un collegamento a Test. Cosa è successo? Se non esiste alcuna pagina denominata SomePage.aspx, quindi si è verificato un errore 404, ma che non è importante qui. Al contrario, lo stato attivo nella barra degli indirizzi del browser. Si noti che include il ticket di autenticazione form nell'URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

SomePage.aspx l'URL nel collegamento è stato automaticamente convertito in un URL che è incluso il ticket di autenticazione - non è necessario scrivere codice. Il ticket di autenticazione form verrà automaticamente incorporato nell'URL per tutti i collegamenti ipertestuali che inizia con http:// o /. Non è importante se il collegamento ipertestuale viene visualizzato in una chiamata a Response. Redirect, in un controllo collegamento ipertestuale o un elemento ancoraggio HTML (ad esempio, &lt;href = "…"&gt;... &lt;/a&gt;). Fino a quando l'URL non è un elemento come http://www.someserver.com/SomePage.aspx o /SomePage.aspx, il ticket di autenticazione form verrà incorporato per noi.

> [!NOTE]
> Ticket di autenticazione form senza cookie rispettare gli stessi criteri di timeout ticket di autenticazione basato su cookie. Tuttavia, il ticket di autenticazione senza cookie sono più inclini a attacchi di tipo replay poiché il ticket di autenticazione è incorporato direttamente nell'URL. Si supponga un utente visita un sito Web, accesso e quindi Incolla l'URL in un messaggio di posta elettronica a un collega. Se il collega fa clic sul collegamento prima che venga raggiunta la scadenza, questi verranno registrati come l'utente che ha inviato il messaggio di posta elettronica.


## <a name="step-3-securing-the-authentication-ticket"></a>Passaggio 3: Protezione il Ticket di autenticazione

Il ticket di autenticazione form viene trasmesso in un cookie o incorporato direttamente all'interno dell'URL. Oltre alle informazioni di identità, il ticket di autenticazione può includere anche dati utente (come verrà illustrato nel passaggio 4). Di conseguenza, è importante che i dati del ticket sono crittografati da intrusi e (cosa più importante) che il sistema di autenticazione form in grado di garantire che il ticket non è stato manomesso.

Per garantire la privacy dei dati del ticket, il sistema di autenticazione form possibile crittografare i dati del ticket. Crittografare i dati del ticket invia informazioni potenzialmente riservate nella rete in testo normale.

Per garantire l'autenticità del ticket, il sistema di autenticazione form deve *convalidare* il ticket. Convalida consiste nel garantire che una particolare parte di dati non è stata modificata e viene eseguita tramite un  *[messaggio il codice di autenticazione (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. In breve, il MAC è una piccola parte di informazioni che identificano i dati che devono essere convalidata (in questo caso, il ticket). Se vengono modificati i dati rappresentati da MAC, quindi MAC e i dati verranno non corrispondano. Inoltre, è difficile termini di calcolo per un pirata informatico sia modificare i dati e generare il proprio MAC per la corrispondenza con i dati modificati.

Quando la creazione (o la modifica) un ticket, il sistema di autenticazione form crea un MAC e lo collega a dati del ticket. Quando arriva una richiesta successiva, il sistema di autenticazione form confronta i dati di MAC e il ticket per convalidare l'autenticità dei dati del ticket. La figura 3 illustra graficamente il flusso di lavoro.


[![Autenticità del Ticket viene garantita tramite un MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Figura 03**: viene garantita autenticità del Ticket tramite un MAC ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


Le misure di sicurezza vengono applicate al ticket di autenticazione dipende dall'impostazione di protezione nel &lt;form&gt; elemento. L'impostazione di protezione può essere assegnato a uno dei tre valori seguenti:

- All: il ticket è sia crittografato e firmato digitalmente (impostazione predefinita).
- Crittografia - solo la crittografia viene applicata, non viene generato alcun MAC.
- None: il ticket viene crittografato né firmato digitalmente.
- Convalida: un MAC viene generata, ma i dati del ticket viene inviati in rete in testo normale.

Microsoft consiglia vivamente utilizzando tutte le impostazioni.

### <a name="setting-the-validation-and-decryption-keys"></a>Impostazione delle chiavi di decrittografia e convalida

La crittografia e hash di algoritmi utilizzati dal sistema di autenticazione form per crittografare e convalidare il ticket di autenticazione sono personalizzabili tramite il [ &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx) in Web. config. Tabella 2 contorni di &lt;machineKey&gt; attributi dell'elemento e i relativi valori possibili.

| **Attributo** | **Descrizione** |
| --- | --- |
| decrittografia | Indica l'algoritmo utilizzato per la crittografia. Questo attributo può avere uno dei quattro valori seguenti: - Auto - impostazione predefinita. Determina l'algoritmo in base alla lunghezza dell'attributo decryptionKey. -AES - utilizza il [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmo. -DES - utilizza il [Data Encryption Standard (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) questo algoritmo è considerato vulnerabile calcoli complessi e non deve essere utilizzato. -3DES - utilizza il [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmo, che applica l'algoritmo DES tre volte. |
| decryptionKey | La chiave privata usata dall'algoritmo di crittografia. Questo valore deve essere una stringa esadecimale di lunghezza appropriata (in base al valore di decrittografia), genera automaticamente o il valore aggiunto, IsolateApps. Aggiunta IsolateApps indica ad ASP.NET di utilizzare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |
| convalida | Indica l'algoritmo utilizzato per la convalida. Questo attributo può avere uno dei quattro valori seguenti: - AES - Usa l'algoritmo Advanced Encryption Standard (AES). -MD5 - utilizza il [Message Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmo. -SHA1 - utilizza il [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmo (impostazione predefinita). -3DES - Usa l'algoritmo Triple DES. |
| validationKey | La chiave privata usata dall'algoritmo di convalida. Questo valore deve essere una stringa esadecimale di lunghezza appropriata (in base al valore in fase di convalida), genera automaticamente o il valore aggiunto, IsolateApps. Aggiunta IsolateApps indica ad ASP.NET di utilizzare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |

**Tabella 2**: il &lt;machineKey&gt; attributi elemento

Una trattazione completa di queste opzioni di crittografia e convalida e i professionisti e gli svantaggi dei vari algoritmi, non rientra nell'ambito di questa esercitazione. Per un'analisi approfondita esaminare questi problemi, incluse indicazioni su quale gli algoritmi di crittografia e convalida da utilizzare, le lunghezze di chiave da usare e il modo migliore per la generazione di queste chiavi, fare riferimento a *Professional ASP.NET 2.0 sicurezza, l'appartenenza e gestione dei ruoli* .

Per impostazione predefinita, le chiavi usate per la crittografia e convalida vengono generate automaticamente per ogni applicazione e tali chiavi vengono archiviate in Autorità di sicurezza locale (LSA). In breve, le impostazioni predefinite garantiscono che le chiavi univoche di un server web dal server web e applicazioni per l'applicazione. Di conseguenza, questo comportamento predefinito non funzionerà per i due scenari seguenti:

- **Web farm** : in un [farm web](http://en.wikipedia.org/wiki/Web_farm) scenario, una singola applicazione web è ospitato in più server web ai fini della scalabilità e ridondanza. Ogni richiesta in ingresso viene inviato a un server nella farm, vale a dire tutta la durata della sessione dell'utente, diversi server può essere utilizzato per gestire le varie richieste. Di conseguenza, ogni server deve utilizzare le stesse chiavi di crittografia e convalida in modo che il ticket di autenticazione form creato, crittografato e convalidato in un server può essere decrittografato e convalidato su un altro server nella farm.
- **Tra l'applicazione di condivisione Ticket** -un singolo server web può ospitare più applicazioni ASP.NET. Se è necessario per queste applicazioni di condividere un ticket di autenticazione form singolo diverse, è fondamentale che le chiavi di crittografia e convalida corrispondano.

Quando si lavora in una web farm, l'impostazione o condividere il ticket di autenticazione tra più applicazioni sullo stesso server, sarà necessario configurare il &lt;machineKey&gt; elemento applicazioni interessate in modo che i relativi decryptionKey e i valori di validationKey individuare le corrispondenze.

Anche se nessuno degli scenari precedenti viene applicata per l'applicazione di esempio, è possibile specificare valori di validationKey e decryptionKey esplicita e definire gli algoritmi da utilizzare. Aggiungere un &lt;machineKey&gt; impostazione nel file Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Per ulteriori informazioni consultare [procedura: configurare MachineKey in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> I valori decryptionKey e validationKey sono stati ricavati dalla [Steve Gibson](http://www.grc.com/stevegibson.htm)del [pagina password perfetto](https://www.grc.com/passwords.htm), che genera l'errore di 64 caratteri esadecimali casuali per visitare ogni pagina. Per ridurre la probabilità di queste chiavi effettua arrivare in applicazioni di produzione, si consiglia di sostituire le chiavi precedenti con quelli generati in modo casuale dalla pagina password perfetto.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Passaggio 4: Archiviazione dei dati utente aggiuntive nel Ticket

Molte applicazioni web su cui visualizzare informazioni o la visualizzazione della pagina di base per l'utente attualmente connesso. Ad esempio, una pagina web potrebbero essere il nome dell'utente e la data in cui che ha effettuato l'ultimo accesso nell'angolo superiore di ogni pagina. Il ticket di autenticazione form Memorizza nome utente dell'utente attualmente connesso, ma quando qualsiasi altra informazione è necessaria, la pagina deve essere eseguita all'archivio dell'utente, in genere un database - per cercare le informazioni non archiviate nel ticket di autenticazione.

Con un minimo di codice che è possibile archiviare informazioni utente aggiuntive nel ticket di autenticazione form. Tali dati possono espressi tramite il [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)del [proprietà UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Si tratta di un ideale per contenere piccole quantità di informazioni sull'utente che è in genere necessaria. Configurazione del sistema di autenticazione form in base al valore specificato in UserData proprietà, è incluso come parte del cookie di ticket di autenticazione e, come gli altri campi di ticket è crittografata e convalidata. Per impostazione predefinita, i dati utente è una stringa vuota.

Per archiviare i dati dell'utente nel ticket di autenticazione, è necessario scrivere codice nella pagina di accesso che acquisisce le informazioni specifiche dell'utente e la archivia nel ticket. Poiché UserData è una proprietà di tipo stringa, i dati in essa archiviati devono essere serializzati in modo corretto sotto forma di stringa. Ad esempio, si supponga che l'archivio utente, inclusi data di nascita di ogni utente e il nome dell'azienda e si desidera archiviare questi due valori di proprietà nel ticket di autenticazione. È stato possibile serializzare questi valori in una stringa concatenando la data dell'utente della stringa di nascita con una barra verticale (|), seguito dal nome del datore di lavoro. Per un utente nato 15 agosto 1974 utilizzabile per Northwind Traders, è necessario assegnare la proprietà UserData la stringa: 1974-08-15 | Northwind Traders.

Ogni volta che è necessario accedere ai dati archiviati nel ticket, è possibile eseguire il selezionandola FormsAuthenticationTicket della richiesta corrente e la deserializzazione della proprietà UserData. Nel caso la data di nascita e datore di lavoro di esempio di nome, è necessario suddividere la stringa di UserData in due sottostringhe in base al delimitatore (|).


[![Informazioni utente aggiuntive possono essere archiviati nel Ticket di autenticazione](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Figura 04**: aggiuntive utente le informazioni possono essere archiviati nel Ticket di autenticazione ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>La scrittura di informazioni per i dati utente

Sfortunatamente, aggiunta di informazioni specifiche dell'utente a un ticket di autenticazione form non è così semplice come ci si aspetterebbe. La proprietà UserData della classe FormsAuthenticationTicket è di sola lettura e può essere specificata solo tramite il costruttore della classe FormsAuthenticationTicket. Quando si specifica la proprietà UserData nel costruttore, è inoltre necessario fornire il ticket's altri valori: il nome utente, la data di emissione, la scadenza e così via. Quando la pagina di accesso è stato creato nell'esercitazione precedente, questa è stata tutti gestita automaticamente dalla classe FormsAuthentication. Quando si aggiunge UserData FormsAuthenticationTicket, è necessario scrivere codice per replicare la maggior parte delle funzionalità già fornita dalla classe FormsAuthentication.

Esaminiamo il codice necessario per l'utilizzo di dati utente aggiornando la pagina Login.aspx per registrare informazioni aggiuntive relative all'utente il ticket di autenticazione. Si supponga che l'archivio utente contiene informazioni su della società per cui lavora l'utente e al titolo e che si desidera acquisire queste informazioni nel ticket di autenticazione. Aggiornamento LoginButton evento Click della pagina Login.aspx in modo che il codice è simile al seguente:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Esaminiamo questo codice una riga alla volta. Il metodo avvia definendo quattro matrici di stringhe: gli utenti, password, companyName e titleAtCompany. Queste matrici contengono i nomi utente, password, i nomi di società e titoli per gli account utente nel sistema, di cui sono disponibili tre: Scott Jisun e Sam. In un'applicazione reale, tali valori verrà richiesta dall'archivio dell'utente, non a livello di codice nel codice sorgente della pagina.

Nell'esercitazione precedente, se le credenziali specificate non sono valide sono chiamati semplicemente FormsAuthentication (UserName.Text, RememberMe.Checked), che i seguenti passaggi:

1. Creazione di moduli di ticket di autenticazione
2. Scrittura il ticket in archivio appropriato. Per i ticket di autenticazione basato su cookie, viene utilizzato insieme i cookie del browser; per i ticket di autenticazione senza cookie, i dati del ticket viene serializzati nell'URL
3. Reindirizzato l'utente alla pagina appropriata

Questi passaggi vengono replicati nel codice precedente. In primo luogo, la stringa che alla fine verrà archiviato nella proprietà UserData è formata combinando il nome della società e il titolo, che delimitano i due valori con un carattere di barra verticale (|).

Dim userDataString As String = String.Concat(companyName(i), "|", titleAtCompany(i))

Successivamente, il metodo viene richiamato, FormsAuthentication.GetAuthCookie che crea il ticket di autenticazione, crittografa e la convalida in base alle impostazioni di configurazione e lo inserisce in un oggetto HttpCookie.

Dim authCookie As HttpCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked)

Per utilizzare il FormAuthenticationTicket incorporato all'interno del cookie, è necessario chiamare la classe di FormAuthentication [decrittografare metodo](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), passando il valore del cookie.

Dim il ticket FormsAuthenticationTicket come = FormsAuthentication.Decrypt(authCookie.Value)

È quindi possibile creare un *nuova* istanza FormsAuthenticationTicket in base ai valori del FormsAuthenticationTicket esistente. Tuttavia, questo nuovo ticket include le informazioni specifiche dell'utente (userDataString).

Dim newTicket As FormsAuthenticationTicket = New FormsAuthenticationTicket(ticket.Version, ticket.Name, ticket.IssueDate, ticket.Expiration, ticket.IsPersistent, userDataString)

È quindi crittografare (e convalidare) la nuova istanza FormsAuthenticationTicket chiamando il [crittografare metodo](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)e ripristinare i dati crittografati (e convalidati) di authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

Infine, authCookie viene aggiunto alla raccolta Cookies e il metodo GetRedirectUrl viene chiamato per determinare la pagina appropriata per inviare all'utente.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Tutto il codice è necessaria perché la proprietà UserData è di sola lettura e la classe FormsAuthentication non fornisce alcun metodo per specificare le informazioni UserData nei metodi GetAuthCookie, SetAuthCookie o RedirectFromLoginPage.

> [!NOTE]
> Il codice che viene esaminato solo archivia informazioni specifiche dell'utente in un ticket di autenticazione basato su cookie. Le classi responsabile della serializzazione il ticket di autenticazione form all'URL sono interne a .NET Framework. In breve, è possibile archiviare i dati utente in un ticket di autenticazione form senza cookie.


### <a name="accessing-the-userdata-information"></a>L'accesso alle informazioni UserData

A questo punto il nome della società e titolo di ogni utente è archiviato nella proprietà UserData del ticket di autenticazione dei form quando l'accesso. Queste informazioni sono accessibili dal ticket di autenticazione in qualsiasi pagina senza un trip all'archivio dell'utente. Per illustrare come queste informazioni possono essere recuperate dalla proprietà UserData, si aggiorna Default.aspx in modo che il messaggio di benvenuto include non solo il nome dell'utente, ma anche la società che lavorano per e al titolo.

Attualmente, Default.aspx contiene un pannello AuthenticatedMessagePanel con un controllo etichetta denominato WelcomeBackMessage. Questo pannello è visualizzato solo per gli utenti autenticati. Aggiornare il codice nella pagina del aspx\_caricare il gestore eventi in modo che risulti simile al seguente:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Se Request.IsAuthenticated è True, quindi proprietà Text del WelcomeBackMessage viene innanzitutto impostata su Bentornato, *username*. Quindi, la proprietà User. Identity viene eseguito il cast a un oggetto FormsIdentity in modo che sia possibile accedere FormsAuthenticationTicket sottostante. Una volta ottenuto il FormsAuthenticationTicket, abbiamo deserializzare la proprietà UserData nel nome della società e il titolo. Questa operazione viene eseguita suddividendo la stringa sul carattere di barra verticale. Nell'etichetta WelcomeBackMessage verranno quindi visualizzati il nome della società e il titolo.

Figura 5 mostra una schermata di questa visualizzazione in azione. Accedere come Scott Visualizza un messaggio di back-benvenuto che include Scott aziendale e il titolo.


[![Vengono visualizzati l'attualmente registrati dell'utente aziendale e il titolo](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Figura 05**: vengono visualizzati il attualmente registrati dell'utente aziendale e il titolo ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> Proprietà UserData del ticket di autenticazione funge da una cache per l'archivio dell'utente. Ad esempio una cache, deve essere aggiornato quando i dati sottostanti viene modificati. Ad esempio, se è presente una pagina web da cui gli utenti possono aggiornare il proprio profilo, i campi memorizzati nella cache nella proprietà UserData devono essere aggiornati per riflettere le modifiche apportate dall'utente.


## <a name="step-5-using-a-custom-principal"></a>Passaggio 5: Utilizzo di un'entità personalizzata

Ogni richiesta in ingresso FormsAuthenticationModule tenta di autenticare l'utente. Se è presente un ticket di autenticazione non scadute, FormsAuthenticationModule assegna la proprietà HttpContext a un nuovo oggetto GenericPrincipal. Questo oggetto GenericPrincipal ha un'identità di tipo FormsIdentity, che include un riferimento a ticket di autenticazione dei form. La classe GenericPrincipal contiene bare funzionalità minime necessarie per una classe che implementa IPrincipal - che dispone di una proprietà Identity e un metodo IsInRole.

L'oggetto principal ha due responsabilità: per indicare i ruoli a cui appartiene l'utente e per fornire informazioni di identità. Questa operazione viene eseguita tramite IsInRole dell'interfaccia IPrincipal (*roleName*) metodo e proprietà Identity, rispettivamente. La classe GenericPrincipal consente di una matrice di stringhe di nomi di ruoli per essere specificato tramite il relativo costruttore. relativo IsInRole (*roleName*) metodo semplicemente controlla se il valore passato in *roleName* esiste all'interno della matrice di stringa. Quando FormsAuthenticationModule crea GenericPrincipal, passa una matrice di stringa vuota al costruttore dell'oggetto GenericPrincipal. Di conseguenza, qualsiasi chiamata a IsInRole restituirà sempre False.

La classe GenericPrincipal soddisfa le esigenze per la maggior parte degli scenari di autenticazione basata su form in cui non vengono utilizzati i ruoli. Per i casi in cui non è sufficiente la gestione di ruolo predefinita o quando è necessario associare un oggetto di identità personalizzato all'utente, è possibile creare un oggetto IPrincipal personalizzato durante il flusso di lavoro di autenticazione e assegnarlo alla proprietà HttpContext.

> [!NOTE]
> Come verrà illustrato in futuro, esercitazioni quando ASP. Del ruoli .NET framework è abilitata viene creato un oggetto personalizzato dell'entità di tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) e sovrascrive l'oggetto GenericPrincipal creato autenticazione form. Ciò avviene per personalizzare il metodo IsInRole dell'entità per l'interazione con l'API del framework di ruoli.


Poiché è stata non interessato effettuata con i ruoli ancora, potrebbe essere l'unico motivo per cui che abbiamo per la creazione di un'entità personalizzata a questo punto di associare un oggetto di identità personalizzato per l'entità. Nel passaggio 4 è stato esaminato l'archiviazione delle informazioni utente aggiuntive nella proprietà UserData del ticket di autenticazione, in particolare il nome dell'utente aziendale e al titolo. Tuttavia, le informazioni di UserData sono solo accessibili tramite il ticket di autenticazione e quindi solo come una stringa serializzata, vale a dire che è necessario analizzare la proprietà UserData ogni volta che si desidera visualizzare le informazioni utente archiviate nel ticket.

Creando una classe che implementa IIdentity e include proprietà CompanyName e titolo migliorare l'esperienza dello sviluppatore. In questo modo, uno sviluppatore possa accedere a nome della società dell'utente attualmente connesso e titolo direttamente tramite le proprietà CompanyName e titolo senza sapere analizzare la proprietà UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Creazione dell'identità personalizzato e classi di entità

Per questa esercitazione, creare gli oggetti principal e identity personalizzati nell'App\_cartella del codice. Per iniziare, aggiungere un'App\_cartella al progetto del codice - pulsante destro del mouse sul nome del progetto in Esplora soluzioni, selezionare l'opzione Aggiungi cartella ASP.NET e scegliere App\_codice. L'App\_cartella del codice è una cartella speciale di ASP.NET che contiene file di classe specifico per il sito Web.

> [!NOTE]
> L'App\_cartella del codice deve essere utilizzata solo quando la gestione del progetto tramite il modello di progetto di sito Web. Se si utilizza il [il modello di progetto applicazione Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), creare una cartella standard e aggiungere le classi che. Ad esempio, è possibile aggiungere una nuova cartella denominata classi e inserire il codice non esiste.


Successivamente, aggiungere due nuovi file di classe per l'App\_cartella del codice, CustomIdentity.vb denominata uno e uno denominato CustomPrincipal.vb.


[![Aggiungere tutte le classi CustomPrincipal CustomIdentity al progetto](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Figura 06**: aggiungere tutte le classi CustomPrincipal CustomIdentity a un progetto ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


La classe CustomIdentity è responsabile dell'implementazione dell'interfaccia di identità, che definisce le proprietà AuthenticationType IsAuthenticated e nome. Oltre a queste proprietà richieste, si intende esporre il sottostante ticket di autenticazione form, nonché le proprietà per il nome di società e il titolo dell'utente. Immettere il codice seguente nella classe CustomIdentity.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Si noti che la classe include una variabile membro FormsAuthenticationTicket (\_ticket) e che queste informazioni ticket devono essere fornite mediante il costruttore. Questi dati del ticket viene utilizzati nel restituire il nome dell'identità. la proprietà UserData viene analizzata per restituire i valori per le proprietà CompanyName e il titolo.

Successivamente, creare la classe CustomPrincipal. Poiché non si è interessati, con i ruoli a questo punto, il costruttore della classe CustomPrincipal accetta solo un oggetto CustomIdentity. il metodo IsInRole restituisce sempre False.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>L'assegnazione di un oggetto CustomPrincipal al contesto di sicurezza della richiesta in ingresso

È ora disponibile una classe che estende la specifica di identità predefinita per includere proprietà CompanyName e Title, nonché una classe di entità personalizzata che utilizza l'identità personalizzata. Si è pronti per passaggio nella pipeline ASP.NET e assegna l'oggetto principal personalizzato al contesto di sicurezza della richiesta in ingresso.

La pipeline ASP.NET accetta una richiesta in ingresso e lo elabora mediante una serie di passaggi. In ogni fase, viene generato un evento specifico, consentendo agli sviluppatori di usufruire della pipeline ASP.NET e modificare la richiesta in determinati momenti nel ciclo di vita. FormsAuthenticationModule, ad esempio, attende ASP.NET generare il [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), a quel punto analizza la richiesta in ingresso per un ticket di autenticazione. Se viene trovato un ticket di autenticazione, un oggetto GenericPrincipal verrà creato e assegnato alla proprietà HttpContext.

Dopo l'evento, AuthenticateRequest della pipeline ASP.NET genera il [evento PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), che in cui è possibile sostituire gli oggetti GenericPrincipal creati dalle FormsAuthenticationModule con un'istanza di questo Oggetto CustomPrincipal. Figura 7 illustra il flusso di lavoro.


[![GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Figura 07**: il GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


Per eseguire codice in risposta a un evento di pipeline ASP.NET, è possibile creare il gestore eventi appropriato in Global. asax o creare un modulo HTTP. Per questa esercitazione consente di creare il gestore dell'evento in Global. asax. Per iniziare, aggiungere Global. asax per il sito Web. Fare clic sul nome del progetto in Esplora soluzioni e aggiungere un elemento di tipo classe di applicazione globale denominato Global. asax.


[![Aggiungere un File Global. asax per il sito Web](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Figura 08**: aggiungere un File Global. asax per il sito Web ([fare clic per visualizzare l'immagine ingrandita](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


Il modello di Global. asax predefinito include i gestori eventi per un numero di eventi della pipeline ASP.NET, tra cui inizio, fine e [evento di errore](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), tra gli altri. È possibile rimuovere questi gestori eventi, come abbiamo non è necessario per questa applicazione. L'evento che si è interessati è PostAuthenticateRequest. Aggiornare il file Global. asax in modo che il tag è simile al seguente:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

L'applicazione\_OnPostAuthenticateRequest metodo viene eseguito ogni volta che il runtime di ASP.NET genera l'evento PostAuthenticateRequest, situazione che si verifica una volta per ogni richiesta di pagina in ingresso. Il gestore dell'evento viene avviato da un controllo per verificare se l'utente viene autenticato e se è stato autenticato tramite l'autenticazione basata su form. In questo caso, un nuovo oggetto CustomIdentity è creato e passato al costruttore ticket di autenticazione della richiesta corrente. Successivamente, un oggetto CustomPrincipal è creato e passato l'oggetto CustomIdentity appena creato nel relativo costruttore. Infine, il contesto di sicurezza della richiesta corrente viene assegnato all'oggetto CustomPrincipal appena creato.

Si noti che l'ultimo passaggio, associarlo a un oggetto CustomPrincipal il contesto di sicurezza della richiesta - assegna l'entità a due proprietà: HttpContext e thread. CurrentPrincipal. Queste due attività sono necessarie a causa del modo in cui vengono gestiti i contesti di sicurezza in ASP.NET. Un contesto di sicurezza di .NET Framework associa ogni thread in esecuzione. Queste informazioni sono disponibili come un oggetto IPrincipal attraverso il [oggetto Thread](https://msdn.microsoft.com/library/system.threading.thread.aspx)del [proprietà CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Che cos'è un chiaro è ASP.NET con le proprie informazioni di contesto di sicurezza (HttpContext).

In alcuni scenari, la proprietà del thread. CurrentPrincipal viene esaminata per determinare il contesto di sicurezza. in altri scenari, viene utilizzato HttpContext. Ad esempio, sono disponibili funzionalità di sicurezza di .NET che consentono agli sviluppatori di dichiarare in modo dichiarativo quali utenti o ruoli è possano creare un'istanza di una classe o richiamare metodi specifici (vedere [aggiungendo le regole di autorizzazione Business e l'utilizzo di livelli di dati PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Sotto le quinte, queste tecniche dichiarative determinano il contesto di sicurezza tramite la proprietà del thread. CurrentPrincipal.

In altri scenari, viene utilizzata la proprietà HttpContext. Ad esempio, nell'esercitazione precedente abbiamo utilizzato questa proprietà per visualizzare il nome utente dell'utente attualmente connesso. Chiaramente, quindi, è fondamentale che le informazioni sul contesto di sicurezza nelle proprietà del thread. CurrentPrincipal e HttpContext corrisponde.

Il runtime di ASP.NET viene sincronizzato automaticamente i valori delle proprietà per noi. Tuttavia, la sincronizzazione si verifica dopo l'evento AuthenticateRequest, ma *prima* l'evento PostAuthenticateRequest. Di conseguenza, quando si aggiunge un'entità personalizzata nell'evento PostAuthenticateRequest è necessario assicurarsi di assegnare manualmente il thread. CurrentPrincipal altrimenti CurrentPrincipal e HttpContext sarà sincronizzato. Vedere [vs Context. User. Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) per informazioni più dettagliate su questo problema.

### <a name="accessing-the-companyname-and-title-properties"></a>L'accesso al CompanyName e proprietà titolo

Ogni volta che una richiesta arriva e viene inviata al motore di ASP.NET, l'applicazione\_verrà generato il gestore eventi OnPostAuthenticateRequest in Global. asax. Se la richiesta è stata autenticata da FormsAuthenticationModule, il gestore dell'evento creerà un nuovo oggetto CustomPrincipal con un oggetto CustomIdentity basato su ticket di autenticazione dei form. Con questa logica sul posto, è molto semplice l'accesso alle informazioni sul nome della società e il titolo dell'utente attualmente connesso.

Tornare alla pagina\_gestore di eventi di caricamento in Default.aspx, in cui nel passaggio 4 è scritto codice per recuperare il ticket di autenticazione form e analizzare la proprietà UserData per visualizzare il nome di società e il titolo dell'utente. Con gli oggetti CustomPrincipal e CustomIdentity in uso a questo punto, non è necessario analizzare i valori dalla proprietà UserData del ticket. È sufficiente ottenere un riferimento all'oggetto CustomIdentity e utilizzare proprietà CompanyName e titolo:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Riepilogo

In questa esercitazione sono state esaminate come personalizzare le impostazioni del sistema di autenticazione form tramite Web. config. È stato esaminato la modalità di gestione di scadenza del ticket di autenticazione e l'utilizzo di crittografia e convalida le misure di sicurezza per proteggere il ticket dall'analisi e la modifica. Infine, abbiamo parlato utilizzando proprietà UserData del ticket di autenticazione per archiviare le informazioni utente aggiuntive nel ticket di se stesso e come utilizzare oggetti principal e identity personalizzati per esporre queste informazioni in modo più semplice.

Questa esercitazione si conclude l'esame dell'autenticazione basata su form in ASP.NET. L'esercitazione successiva avvia nostro viaggio nel framework di appartenenza.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Sezionando l'autenticazione basata su form](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Spiegazione: Autenticazione basata su form in ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Procedura: Proteggere l'autenticazione basata su form in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 sicurezza, l'appartenenza e gestione dei ruoli](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
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

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Alicja Maziarz. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Precedente](an-overview-of-forms-authentication-vb.md)
