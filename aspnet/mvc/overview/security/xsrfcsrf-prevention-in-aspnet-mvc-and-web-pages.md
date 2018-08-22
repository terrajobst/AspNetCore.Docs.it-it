---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevenzione di XSRF/CSRF in ASP.NET MVC e pagine Web | Microsoft Docs
author: Rick-Anderson
description: Richiesta intersito falsa (nota anche come XSRF o CSRF) è un attacco contro applicazioni ospitate sul web in base al quale un sito web dannoso può influenzare il interacti...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: cd1b8de51c180471ab273c4541959368ffbd48a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826393"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevenzione di XSRF/CSRF in ASP.NET MVC e pagine Web
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Richiesta intersito falsa (nota anche come XSRF o CSRF) è un attacco contro applicazioni ospitate sul web in base al quale un sito web dannoso può influenzare l'interazione tra un browser client e un sito web considerato attendibile da tale browser. Questi attacchi sono possibili in quanto i web browser invia i token di autenticazione automaticamente con ogni richiesta a un sito web. L'esempio canonico è un cookie di autenticazione, ad esempio, ASP. Ticket di autenticazione basata su form di NET. Tuttavia, siti web che utilizzano qualsiasi meccanismo di autenticazione persistente (ad esempio l'autenticazione di Windows, Basic e così via) di destinazione per questi attacchi.
> 
> Un attacco XSRF è diverso da un attacco di phishing. Gli attacchi di phishing richiedono l'interazione da parte della vittima. In un attacco di phishing, un sito web dannoso imita il sito web di destinazione e la vittima viene indotta a fornire le informazioni riservate all'autore dell'attacco. In un attacco XSRF non è spesso interagisce necessaria da parte della vittima. Piuttosto, l'autore dell'attacco si basa il browser invii automaticamente tutti i cookie pertinenti al sito web di destinazione.
> 
> Per altre informazioni, vedere la [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomia di un attacco

Per spostarsi all'interno di un attacco XSRF, prendere in considerazione un utente che si desidera eseguire alcune operazioni di banking online. L'utente visita prima WoodgroveBank.com e i log, a quel punto l'intestazione della risposta conterrà il cookie di autenticazione:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Poiché il cookie di autenticazione è un cookie di sessione, si verrà cancellato automaticamente dal browser alla chiusura del processo del browser. Tuttavia, fino a quel momento, il browser includerà automaticamente i cookie con ogni richiesta a WoodgroveBank.com. A questo punto, l'utente vuole trasferire 1000 dollari per un altro account, in modo che Anna compila un modulo nel sito di servizi bancari, e il browser invia la richiesta al server:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Poiché questa operazione ha un effetto collaterale (all'avvio di una transazione monetaria), il sito di servizi bancari ha scelto di richiedere una richiesta HTTP POST per avviare l'operazione. Il server legge il token di autenticazione della richiesta, Cerca il numero di account dell'utente corrente, verifica che i fondi sufficienti esiste e quindi avvia la transazione nell'account di destinazione.

L'utente online banking completo, l'utente visita il sito di servizi bancari e visita altri percorsi sul web. Uno di quei siti – fabrikam.com – include il markup seguente in una pagina incorporata all'interno di un' &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Che a sua fa sì che il browser effettuare questa richiesta:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

L'autore dell'attacco sta sfruttando il fatto che l'utente potrebbe avere ancora un token di autenticazione valido per il sito web di destinazione e Anna Usa un piccolo frammento di codice di Javascript per fare in modo il browser apportare automaticamente una richiesta HTTP POST al sito di destinazione. Se il token di autenticazione è ancora valido, il sito di servizi bancari avvierà un trasferimento di $250 conto della scelta dell'utente malintenzionato.

### <a name="ineffective-mitigations"></a>Mitigazioni inefficaci

È interessante notare che nello scenario precedente, il fatto che WoodgroveBank.com è stato possibile accedere tramite SSL e ha un cookie di autenticazione solo SSL è stato sufficiente per contrastare l'attacco. L'autore dell'attacco è in grado di specificare il [schema URI](http://en.wikipedia.org/wiki/URI_scheme) (https) nel proprio &lt;form&gt; elemento e il browser continuerà a inviare i cookie non scaduti al sito di destinazione, purché i cookie sono coerenti con l'URI schema di destinazione prevista.

Si potrebbe sostenere che l'utente deve semplicemente non visitare siti non attendibili, come visitato solo siti attendibili è consente di rimanere sicuri. Ecco alcuni dati reali a questo, ma purtroppo questa raccomandazione non è sempre pratica. Ad esempio l'utente "considera attendibile" il sito di notizie locali ConsolidatedMessenger. Una vulnerabilità XSS che consente a un utente malintenzionato di inserire lo stesso frammento di codice che era in esecuzione in fabrikam.com ConsolidatedMessenger.com e passa alla visita che invece del sito, ma tale sito.

È possibile verificare che le richieste in ingresso hanno una [intestazione Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) che fa riferimento il dominio. L'operazione arresterà involontariamente autore richieste di un dominio di terze parti. Tuttavia, alcune persone disabilitare intestazione Referer del browser per motivi di privacy e gli utenti malintenzionati possono talvolta effettuare lo spoofing tale intestazione se la vittima è determinate installato software non sicuri. Verifica per determinare se il [intestazione Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) non viene considerato un approccio sicuro per la prevenzione degli attacchi XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Prevenzione di XSRF Runtime Stack Web

ASP.NET Web Stack di Runtime Usa una variante del [modello di token sincronizzatore](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) per difendersi da attacchi XSRF. La forma generale del modello di token del programma di sincronizzazione è che due token anti-XSRF vengano inviate al server con ogni richiesta HTTP POST (oltre al token di autenticazione): un token come un cookie e l'altro come valore di modulo. I valori del token generati dal runtime di ASP.NET non sono deterministiche o stimabile da un utente malintenzionato. Quando vengono inviati i token, il server consentirà alla richiesta di procedere solo se entrambi i token superano un controllo di confronto.

La verifica della richiesta XSRF *token di sessione* viene archiviato come un cookie HTTP e attualmente contiene le informazioni seguenti nel relativo payload:

- Un token di sicurezza, costituito da un identificatore casuale a 128 bit.   
 L'immagine seguente mostra il token di sessione XSRF richiesta verifica visualizzato con strumenti di sviluppo F12 di Internet Explorer: (questo è l'implementazione corrente di soggetto, ancora soggette a modifica.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

Il *token pole* viene archiviato come un `<input type="hidden" />` e contiene le informazioni seguenti nel relativo payload:

- Nome utente dell'utente connesso (se autenticato).
- I dati aggiuntivi forniti da un' [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

I payload dei token anti-XSRF vengono crittografati e firmati, pertanto non è possibile visualizzare il nome utente quando si usano strumenti per esaminare i token. Quando l'applicazione web è la destinazione di ASP.NET 4.0, forniti da servizi di crittografia di [machineKey](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) routine. Quando l'applicazione web è destinato a ASP.NET 4.5 o versione successiva, crittografia servizi vengono forniti dal [machineKey. Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) routine, che offre migliori prestazioni, estendibilità e sicurezza. Il seguente post di blog per altri dettagli, vedere:

- [Miglioramenti crittografici in ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Miglioramenti crittografici in ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Miglioramenti crittografici in ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generazione di token

Per generare i token anti-XSRF, chiamare il [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) metodo da una visualizzazione MVC o @AntiForgery.GetHtml() da una pagina Razor. Il runtime eseguirà quindi i passaggi seguenti:

1. Se la richiesta HTTP corrente contiene già un token di sessione di anti-XSRF (il cookie di anti-XSRF \_ \_RequestVerificationToken), il token di sicurezza verrà estratti da quest'ultimo. Se la richiesta HTTP non contiene un token di sessione di anti-XSRF o estrazione del token di sicurezza non riesce, verrà generato un nuovo token anti-XSRF casuale.
2. Un token di anti-XSRF campo viene generato usando il token di sicurezza dal passaggio (1) e l'identità dell'utente corrente ha eseguito l'accesso. (Per altre informazioni su come determinare l'identità dell'utente, vedere la **[scenari con un supporto speciale](#_Scenarios_with_special)** sezione riportata di seguito.) Inoltre, se un' [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) è configurato, il runtime chiama relativo [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) (metodo) e includere la stringa restituita nel token di campo. (Vedere la **[Configuration ed estendibilità](#_Configuration_and_extensibility)** sezione per altre informazioni.)
3. Se un nuovo token anti-XSRF è stato generato nel passaggio (1), un nuovo token di sessione per contenerlo verrà creato e verrà aggiunto all'insieme di cookie HTTP in uscita. Il token del campo ottenuto nel passaggio (2) verrà eseguito in un' `<input type="hidden" />` elemento e il markup HTML sarà il valore restituito di `Html.AntiForgeryToken()` o `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Convalida i token

Per convalidare il token anti-XSRF in ingresso, lo sviluppatore include un' [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) attributo sul suo azione MVC o controller oppure digita `@AntiForgery.Validate()` dalla propria pagina Razor. Il runtime eseguirà i passaggi seguenti:

1. Il token di sessione in ingresso e un token di campo vengono letti e il token anti-XSRF estratti da ognuna. I token anti-XSRF devono essere identici per ogni passaggio (2) nella routine di generazione.
2. Se l'utente corrente è autenticato, il suo nome utente viene confrontato con il nome utente archiviato nel token di campo. I nomi utente devono corrispondere.
3. Se un' [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) configurato, il runtime chiama relativo *ValidateAdditionalData* (metodo). Il metodo deve restituire il valore booleano *true*.

Se la convalida ha esito positivo, la richiesta può continuare. Se la convalida non riesce, il framework genera una *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condizioni di errore

Inizia con il Runtime di Stack Web ASP.NET v2, qualsiasi *HttpAntiForgeryException* che viene generata un'eccezione durante la convalida conterrà informazioni dettagliate sulle cause dell'errore. Le condizioni di errore attualmente definiti sono:

- Il token di sessione o un token del form non è presente nella richiesta.
- Il token di sessione o un token del form è illeggibile. La causa più probabile di questo oggetto è una farm che eseguono versioni non corrispondenti di The ASP.NET Web Stack di Runtime o in una farm in cui il &lt;machineKey&gt; in Web. config è diverso tra i computer. È possibile utilizzare uno strumento come Fiddler per forzare questa eccezione, la manomissione di uno dei due token anti-XSRF.
- Il token di sessione e token pole sono stati scambiati.
- Il token di sessione e token pole contengono i token di sicurezza non corrispondenti.
- Il nome utente incorporato all'interno del token di campo non corrisponde a username ha eseguito l'accesso dell'utente corrente.
- Il *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* metodo restituito *false*.

Le funzionalità di anti-XSRF possono anche eseguire controlli aggiuntivi durante la generazione di token o la convalida e la generazione di eccezioni possono verificarsi errori durante questi controlli. Vedere la [WIF / ACS / basata su attestazioni authentication](#_WIF_ACS) e **[Configuration ed estendibilità](#_Configuration_and_extensibility)** sezioni per ulteriori informazioni.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scenari con un supporto speciale

### <a name="anonymous-authentication"></a>Autenticazione anonima

Il sistema di anti-XSRF contiene un supporto speciale per gli utenti anonimi, dove "anonymous" è definito come un utente in cui il *IIdentity* restituisce proprietà *false*. Gli scenari includono la protezione XSRF alla pagina di accesso (prima che l'utente viene autenticato) e gli schemi di autenticazione personalizzata in cui l'applicazione usa un meccanismo diverso da *IIdentity* per identificare gli utenti.

Per supportare questi scenari, è importante ricordare che i token di sessione e campo vengono uniti mediante un token di sicurezza, ovvero un identificatore opaco a 128 bit generato in modo casuale. Questo token di sicurezza viene usato per tenere traccia della sessione di un singolo utente come Anna passa il sito, in modo che svolge in modo efficace la funzione di un identificatore anonimo. Una stringa vuota viene usata al posto del nome utente per le routine di convalida e generazione descritte in precedenza.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / basata su attestazioni autenticazione

In genere, il *IIdentity* classi incorporate .NET Framework dispongono della proprietà che *IIdentity.Name* è sufficiente per identificare in modo univoco un utente specifico all'interno di una determinata applicazione. Ad esempio, *FormsIdentity.Name* restituisce il nome utente archiviato nel database delle appartenenze (che è univoco per tutte le applicazioni a seconda di quel database), *WindowsIdentity.Name* restituisce il identità di dominio completo dell'utente e così via. Questi sistemi forniscono non solo l'autenticazione. sono inoltre *identificare* agli utenti di un'applicazione.

L'autenticazione basata su attestazioni, d'altra parte, non necessariamente richiedono l'identificazione di un determinato utente. Al contrario, il *ClaimsPrincipal* e *ClaimsIdentity* tipi sono associati a un set di *attestazione* istanze, in cui le singole attestazioni potrebbero essere "is 18 anni di età" o " è un amministratore"in un account. Poiché l'utente non ha necessariamente stato identificato, il runtime non è possibile usare la *ClaimsIdentity.Name* proprietà come un identificatore univoco per questo utente specifico. Il team ha riscontrato esempi reali in cui *ClaimsIdentity.Name* restituisce *null*, restituisce un nome descrittivo (display) o in caso contrario, restituisce una stringa che non è appropriata per l'utilizzo come un identificatore univoco per l'utente.

Molte delle distribuzioni che usano l'autenticazione basata su attestazioni usano [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) in particolare. Servizio contenitore di AZURE consente allo sviluppatore di configurare singole *provider di identità* (ad esempio ad FS, il provider di Account Microsoft, OpenID provider, ad esempio Yahoo!, e così via), e restituiscono i provider di identità *denominare gli identificatori*. Questi identificatori del nome possono contenere informazioni identificabili personalmente (PII), ad esempio un indirizzo di posta elettronica o potrebbe essere resi anonimi, ad esempio una personale privato identificatore (PPID). Indipendentemente da ciò, la tupla (provider di identità, identificatore nome) sufficientemente funge da un token di rilevamento appropriato per un determinato utente mentre Anna durante l'esplorazione del sito, in modo che il Runtime di ASP.NET Web Stack può utilizzare la tupla al posto del nome utente durante la generazione e la convalida dei token di campo anti-XSRF. Gli URI specifico per il provider di identità e l'identificatore del nome sono:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(vedere questo [pagina di documentazione di ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) per altre informazioni.)

Durante la generazione o la convalida un token, il Runtime di ASP.NET Web Stack in fase di runtime tenterà di associazione ai tipi:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Per il SDK WIF.)
- `System.Security.Claims.ClaimsIdentity` (Per .NET 4.5).

In presenza di questi tipi e se l'utente corrente *IIIIdentity* implementa o alle sottoclassi uno di questi tipi, la funzionalità di anti-XSRF userà (provider di identità, identificatore nome) tuple anziché il nome utente durante la generazione e convalida i token. Se non è presente alcun tale tupla, la richiesta avrà esito negativo con un errore che descrive allo sviluppatore di come configurare il sistema di anti-XSRF per comprendere il meccanismo di autenticazione basata sulle attestazioni specifico in uso. Vedere le **[Configuration ed estendibilità](#_Configuration_and_extensibility)** sezione per altre informazioni.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID autenticazione

Infine, la funzionalità di anti-XSRF include uno speciale supporto per le applicazioni che usano l'autenticazione OAuth o OpenID. Questo supporto è basato sull'approccio euristico: se l'oggetto corrente *IIdentity.Name* inizi con http:// o https://, quindi i confronti di nome utente verranno eseguiti usando un operatore di confronto ordinale anziché l'operatore di confronto OrdinalIgnoreCase.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configurazione ed estendibilità

In alcuni casi, gli sviluppatori potrebbe essere necessario un controllo sulla generazione di anti-XSRF e i comportamenti di convalida. Ad esempio, il comportamento predefinito degli helper MVC e pagine Web di aggiungendo automaticamente i cookie HTTP alla risposta è probabilmente indesiderato e lo sviluppatore può essere utile rendere persistenti i token in un' posizione. Esistono due API per agevolare questo:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

Il *GetTokens* metodo accetta come input un XSRF richiesta verifica sessione token esistente (che può essere null) e produce come output un nuovo token della sessione verifica XSRF richiesta e token pole. I token sono stringhe opache semplicemente senza decorazioni; il *formToken* valore, ad esempio non verrà racchiusi un &lt;input&gt; tag. Il *newCookieToken* valore può essere null; in tal caso, il *oldCookieToken* valore sia ancora valido e nessun nuovo cookie di risposta debba essere impostata. Il chiamante *GetTokens* è responsabile della persistenza di tutti i cookie di risposta necessarie o la generazione di alcun markup necessarie; le *GetTokens* metodo di stesso non modificherà la risposta come un effetto collaterale. Il *Validate* metodo accetta la sessione in ingresso e campo dei token e si esegue la logica di convalida menzionati in precedenza su di essi.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Lo sviluppatore può configurare il sistema di anti-XSRF dall'applicazione\_Start. La configurazione è a livello di codice. Le proprietà di statica *AntiForgeryConfig* tipo sono descritti di seguito. La maggior parte degli utenti che usano attestazioni saranno possibile impostare la proprietà UniqueClaimTypeIdentifier.

| **Property** | **Descrizione** |
| --- | --- |
| **AdditionalDataProvider** | Un' [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) che fornisce dati aggiuntivi durante la generazione di token e utilizza i dati aggiuntivi durante la convalida del token. Il valore predefinito è *null*. Per altre informazioni, vedere la [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) sezione. |
| **CookieName** | Stringa che specifica il nome del cookie HTTP a cui viene usato per archiviare il token di sessione di anti-XSRF. Se questo valore non è impostato, verrà generato un nome da automaticamente in base al percorso virtuale distribuito dell'applicazione. Il valore predefinito è *null*. |
| **RequireSsl** | Valore booleano che determina se il token anti-XSRF sono necessarie da inviare tramite un canale protetto con SSL. Se questo valore è *true*, qualsiasi cookie generato automaticamente verranno impostato il flag "secure" e le API di anti-XSRF genererà se chiamato dall'interno di una richiesta che non viene inviata tramite SSL. Il valore predefinito è *false*. |
| **SuppressIdentityHeuristicChecks** | Valore booleano che determina se il sistema di anti-XSRF verrà disattivato il supporto per identità basate sulle attestazioni. Se questo valore è *true*, il sistema presupporrà che *IIdentity.Name* può essere usata come identificatore utente univoco e non proverà a caso speciale *IClaimsIdentity*oppure *ClClaimsIdentity* come descritto nel [WIF / ACS / basata su attestazioni autenticazione](#_WIF_ACS) sezione. Il valore predefinito è `false`. |
| **UniqueClaimTypeIdentifier** | Una stringa che indica il tipo di attestazione che può essere usata come identificatore univoco per ogni utente. Se questo valore viene impostato e l'oggetto corrente *IIdentity* basata sulle attestazioni, il sistema tenterà di estrarre un'attestazione del tipo specificata da *UniqueClaimTypeIdentifier*, e verrà usato il valore corrispondente al posto di nome utente dell'utente durante la generazione del token di campo. Se il tipo di attestazione non viene trovato, il sistema avrà esito negativo della richiesta. Il valore predefinito è *null*, che indica che il sistema deve utilizzare (provider di identità, identificatore nome) tupla come descritto in precedenza al posto di nome utente dell'utente. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

Il *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* tipo consente agli sviluppatori di estendere il comportamento del sistema anti-XSRF, i dati aggiuntivi di andata e ritorno in ogni token. Il *GetAdditionalData* viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato. Un implementatore potrebbe restituire un timestamp, un parametro nonce o qualsiasi altro valore che Anna vuole che da questo metodo.

Analogamente, il *ValidateAdditionalData* viene chiamato ogni volta che viene convalidato un token di campo e la stringa "dati aggiuntivi" che è stata incorporata all'interno del token viene passata al metodo. La routine di convalida è stato possibile implementare un timeout (controllando l'ora corrente con l'ora in cui è stato archiviato quando è stato creato il token), un controllo della routine o qualsiasi altro parametro nonce desiderate per la logica.

## <a name="design-decisions-and-security-considerations"></a>Decisioni di progettazione e considerazioni sulla sicurezza

Il token di sicurezza che collega i token di sessione e il campo è tecnicamente necessario solo quando si tenta di proteggere gli utenti anonimi o autenticati da attacchi XSRF. Quando l'utente è autenticato, il token di autenticazione stesso (presumibilmente inviati sotto forma di cookie) può essere utilizzato come una coppia di metà di una sincronizzazione del token. Tuttavia, esistono scenari validi per la protezione delle pagine di accesso, l'hit testing, gli utenti non autenticati e la logica di anti-XSRF è stata resa più semplice da sempre la generazione e la convalida del token di sicurezza, anche per gli utenti autenticati. È inoltre fornire un livello di protezione aggiuntiva nel caso in cui un token di campo viene danneggiato da un utente malintenzionato, come l'impostazione o un'ipotesi che il token di sessione sarebbe un altro ostacolo per l'utente malintenzionato superare.

Gli sviluppatori devono prestare attenzione quando più applicazioni sono ospitate in un singolo dominio. Ad esempio, anche se *example1.cloudapp.net* e *example2.cloudapp.net* sono diversi host, c'è una relazione di trust implicita tra tutti gli host sotto il  *\*. cloudapp.net* dominio. Questa relazione di trust implicita [consente agli host potenzialmente non attendibili a hanno effetto sui cookie reciproci](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (lo stessa origine che regolano le richieste AJAX non vengono necessariamente applicati criteri per i cookie HTTP). Il Runtime di ASP.NET Web Stack fornisce alcuni mitigazione in quanto il nome utente viene incorporato nel token di campo, in modo che anche se un sottodominio dannoso è in grado di sovrascrivere un token di sessione non sarà in grado di generare un token di campo valido per l'utente. Tuttavia, quando ospitati in tale ambiente le routine incorporato anti-XSRF ancora non è possibile difendersi dal Hijack della sessione o account di accesso XSRF.

Le routine di anti-XSRF attualmente non difendersi [clickjacking](https://www.owasp.org/index.php/Clickjacking). Le applicazioni che intendo a difendersi contro il clickjacking possono farlo facilmente tramite l'invio di un X-Frame-Options: intestazione SAMEORIGIN con ogni risposta. Questa intestazione è supportata da tutti i browser recenti. Per altre informazioni, vedere la [blog di IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), il [blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), e [OWASP](https://www.owasp.org/index.php/Clickjacking). Il Runtime di ASP.NET Web Stack può in alcuni assicurarsi delle prossime versioni MVC e gli helper di anti-XSRF pagine Web questa intestazione viene impostata automaticamente in modo che le applicazioni vengano protetti automaticamente da questi attacchi.

Gli sviluppatori Web devono continuare ad assicurarsi che il sito non sia vulnerabile ad attacchi XSS. Gli attacchi XSS sono molto potenti, e se l'attacco riesce anche interromperebbe il Runtime di ASP.NET Web Stack difese contro gli attacchi XSRF.

## <a name="acknowledgment"></a>Riconoscimento

[@LeviBroderick](https://twitter.com/LeviBroderick), che ha scritto gran parte del codice di protezione ASP.NET la maggior parte di queste informazioni.
