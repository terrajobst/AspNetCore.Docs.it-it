---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevenzione di XSRF/CSRF in ASP.NET MVC e pagine Web | Documenti Microsoft
author: Rick-Anderson
description: "Falsificazione della richiesta tra siti tra (noto anche come XSRF o CSRF) è un attacco contro applicazioni ospitate da web in base al quale un sito web in grado di influenzare il interacti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 4ff4ed20d0768a48f8afb2deeb7cdb6b4c60b5bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevenzione di XSRF/CSRF in ASP.NET MVC e pagine Web
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> Richiesta intersito forgery (noto anche come XSRF o CSRF) è un attacco contro applicazioni ospitate da web in base al quale un sito web in grado di influenzare l'interazione tra un browser del client e un sito web considerato attendibile da tale browser. Questi attacchi sono possibili perché browser invierà i token di autenticazione automaticamente con ogni richiesta a un sito web. L'esempio canonico è un cookie di autenticazione, ad esempio, ASP. Ticket di autenticazione basata su form della rete. Tuttavia, i siti web che utilizzano qualsiasi meccanismo di autenticazione persistente (ad esempio l'autenticazione di Windows, Basic e così via) può essere assegnato da tali attacchi.
> 
> Un attacco XSRF è diverso da quello di un attacco di phishing. Attacchi di phishing richiedono l'interazione da vittima. In un attacco di phishing, un sito web dannoso imitino il sito web di destinazione e la vittima è significativo a fornire le informazioni riservate all'autore dell'attacco. In un attacco XSRF, non vi è spesso l'intervento del necessari da vittima. Piuttosto, l'autore dell'attacco si basa sul browser automaticamente l'invio di tutti i cookie pertinenti al sito web di destinazione.
> 
> Per ulteriori informazioni, vedere il [Apri progetto di sicurezza dell'applicazione Web](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomia di un attacco di tipo

Per analizzare un attacco XSRF, considerare un utente che desidera eseguire alcune transazioni di banking online. L'utente visita prima WoodgroveBank.com e log, a quel punto l'intestazione della risposta conterrà il cookie di autenticazione:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Poiché il cookie di autenticazione è un cookie di sessione, si verrà cancellato automaticamente dal browser quando il processo di browser viene chiuso. Tuttavia, fino a quel momento, il browser verrà automaticamente incluso il cookie in ciascuna richiesta di WoodgroveBank.com. A questo punto, l'utente desidera trasferire $1000 in un altro account, quindi compila un modulo nel sito di servizi bancari e il browser invia la richiesta al server:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Poiché questa operazione ha un effetto collaterale (Avvia una transazione monetaria), il sito di servizi bancari ha scelto di richiedere un POST HTTP per avviare l'operazione. Il server legge il token di autenticazione della richiesta, Cerca il numero di conto dell'utente corrente, verifica che copertura esiste e quindi avvia la transazione nell'account di destinazione.

Alice online banking online completa, l'utente si sposta dal sito di servizi bancari e visita altri percorsi sul web. Uno di questi siti: fabrikam.com – include il markup seguente in una pagina incorporata all'interno di un &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Che quindi determina il browser effettuare questa richiesta:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

L'autore dell'attacco sta sfruttando il fatto che l'utente potrebbe avere comunque un token di autenticazione valido per il sito web di destinazione e utilizza un piccolo frammento di codice di Javascript per il browser rendere un HTTP POST per il sito di destinazione automaticamente. Se il token di autenticazione è ancora valido, il sito di servizi bancari avvierà un trasferimento di $250 conto della scelta dell'utente malintenzionato.

### <a name="ineffective-mitigations"></a>Misure di attenuazione inefficaci

È interessante notare che nello scenario precedente, il fatto che WoodgroveBank.com si stava accedendo tramite SSL e aveva un cookie di autenticazione solo SSL non è sufficiente per contrastare l'attacco. L'utente malintenzionato è in grado di specificare il [schema URI](http://en.wikipedia.org/wiki/URI_scheme) (https) nel proprio &lt;modulo&gt; elemento e il browser continuerà a inviare i cookie non scaduti per il sito di destinazione, purché i cookie sono coerenti con l'URI schema di destinazione prevista.

È possibile sostenere che l'utente deve semplicemente non visitare siti non attendibili, come visita solo siti attendibili consente di rimanere sicuri. Ecco alcuni dati reali a questo, ma purtroppo questa raccomandazione non è sempre pratica. Ad esempio l'utente "trust" il sito di notizie locali ConsolidatedMessenger. ConsolidatedMessenger.com e si visita un sito Web in alternativa, ma tale sito dispone di una vulnerabilità XSS che consente a un utente malintenzionato di inserire il frammento di codice in esecuzione nel fabrikam.com stesso.

È possibile verificare che le richieste in ingresso abbiano un [intestazione Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) che fa riferimento il dominio. Le richieste inviate involontariamente da un dominio di terze parti verrà interrotta. Tuttavia, alcune persone, disabilitare l'intestazione Referrer del browser per motivi di privacy e gli utenti malintenzionati possono talvolta effettuare lo spoofing intestazione se la vittima dispone di alcuni software installato. Verifica il [intestazione Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) non viene considerato un metodo sicuro per impedire gli attacchi XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Misure di attenuazione Stack Runtime XSRF Web

Il Runtime di ASP.NET Web Stack Usa una variante del [modello alla sincronizzazione del token](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) per difendersi da attacchi XSRF. Il formato generale del modello di token di sincronizzazione è che i due token anti-XSRF vengono inviate al server con ogni HTTP POST (oltre al token di autenticazione): un token come un cookie e l'altro come un valore di modulo. I valori dei token generati dal runtime ASP.NET non sono deterministiche o prevedibile da un utente malintenzionato. Quando vengono inviati i token, il server consentirà alla richiesta di procedere solo se entrambi i token di passare un controllo di confronto.

La verifica di richiesta XSRF *token di sessione* viene archiviato come un cookie HTTP e contiene le informazioni seguenti nel relativo payload:

- Un token di sicurezza, costituito da un identificatore di 128 bit casuale.   
 La figura seguente mostra il token di sessione XSRF richiesta verifica visualizzato con gli strumenti di sviluppo F12 di Internet Explorer: (questo è l'implementazione corrente di oggetto, anche soggette a modifica.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

Il *token di campo* viene archiviato come un `<input type="hidden" />` e contiene le informazioni seguenti nel relativo payload:

- Nome utente dell'utente connesso (se autenticato).
- Eventuali dati aggiuntivi forniti da un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

I payload dei token anti-XSRF vengono crittografati e firmati, pertanto non è possibile visualizzare il nome utente quando si utilizzano strumenti per esaminare i token. Quando l'applicazione web è la destinazione è 4.0 di ASP.NET, servizi di crittografia forniti dal [MachineKey.Encode](https://msdn.microsoft.com/en-us/library/system.web.security.machinekey.encode.aspx) routine. Quando l'applicazione web è destinato a ASP.NET 4.5 o forniti da servizi di crittografia, maggiore di [MachineKey.Protect](https://msdn.microsoft.com/en-us/library/system.web.security.machinekey.protect(v=vs.110)) routine, che offre prestazioni migliori, estensibilità e sicurezza. Post di blog seguente per ulteriori informazioni, vedere:

- [Miglioramenti di crittografia in ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Miglioramenti di crittografia in ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Miglioramenti di crittografia in ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generazione di token

Per generare il token anti-XSRF, chiamare il [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/en-us/library/dd470175.aspx) metodo da una visualizzazione MVC o @AntiForgery.GetHtml() da una pagina Razor. Il runtime eseguirà quindi i passaggi seguenti:

1. Se la richiesta HTTP corrente contiene già un token di sessione di anti-XSRF (il cookie di anti-XSRF \_ \_RequestVerificationToken), il token di sicurezza viene estratto da esso. Se la richiesta HTTP non contiene un token di sessione di anti-XSRF o estrazione del token di sicurezza non riesce, verrà generato un nuovo token anti-XSRF casuale.
2. Un token di campo di anti-XSRF viene generato usando il token di sicurezza dal passaggio (1) e l'identità dell'utente corrente connesso. (Per ulteriori informazioni su come determinare l'identità dell'utente, vedere il  **[scenari con un supporto speciale](#_Scenarios_with_special)**  sezione riportata di seguito.) Inoltre, se un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/jj158328(v=vs.111).aspx) è configurato, il runtime chiamerà relativo [GetAdditionalData](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) (metodo) e includere la stringa restituita nel token di campo. (Vedere il  **[configurazione e l'estensibilità](#_Configuration_and_extensibility)**  sezione per ulteriori informazioni.)
3. Se un nuovo token anti-XSRF è stato generato nel passaggio (1), un token di sessione nuovo verrà creato per contenere la e verrà aggiunto alla raccolta di cookie HTTP in uscita. Il token di campo dal passaggio (2) verrà incapsulato in un `<input type="hidden" />` elemento e il markup HTML sarà il valore restituito di `Html.AntiForgeryToken()` o `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Convalida i token

Per convalidare i token anti-XSRF in ingresso, lo sviluppatore include un [ValidateAntiForgeryToken](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) attributo proprio azione MVC o un controller o un utente e le chiamate `@AntiForgery.Validate()` dalla propria pagina Razor. Il runtime eseguirà i passaggi seguenti:

1. Il token di sessione in ingresso e token di campo vengono lette e il token anti-XSRF estratti da ciascuno. Il token anti-XSRF devono essere identici per ogni passaggio (2) nella routine di generazione.
2. Se l'utente corrente è autenticato, il nome utente viene confrontato con il nome utente archiviato nel token di campo. I nomi utente devono corrispondere.
3. Se un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) è configurato, il runtime chiama il relativo *ValidateAdditionalData* metodo. Il metodo deve restituire il valore booleano *true*.

Se la convalida ha esito positivo, la richiesta può continuare. Se la convalida non riesce, il framework genera un *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condizioni di errore

A partire con il Runtime di Stack Web ASP.NET v2, qualsiasi *HttpAntiForgeryException* generata durante la convalida conterrà informazioni dettagliate sulla causa dell'errore. Le condizioni di errore attualmente definiti sono:

- Il token di sessione o un token del form non è presente nella richiesta.
- Il token di sessione o un token del form è illeggibile. La causa più probabile di questo oggetto è una farm che eseguono versioni non corrispondenti di una farm o di ASP.NET Web Stack di Runtime in cui il &lt;machineKey&gt; elemento nel file Web. config è diverso tra computer. È possibile utilizzare uno strumento come Fiddler per manomissione di uno dei token anti-XSRF forzare questa eccezione.
- Il token di sessione e i token di campo sono stati scambiati.
- Il token di sessione e un token di campo contengono token di sicurezza non corrispondenti.
- Il nome utente incorporato all'interno di token di campo non corrisponde a nome utente di accesso dell'utente corrente.
- Il  *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)*  metodo restituito *false*.

Le funzionalità di anti-XSRF possono inoltre eseguire ulteriori controlli durante la generazione di token o la convalida e potrebbero verificarsi errori durante questi controlli di eccezioni. Vedere il [WIF / ACS / basata sulle attestazioni autenticazione](#_WIF_ACS) e  **[configurazione e l'estensibilità](#_Configuration_and_extensibility)**  sezioni per ulteriori informazioni.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scenari con un supporto speciale

### <a name="anonymous-authentication"></a>Autenticazione anonima

Il sistema di anti-XSRF contiene un supporto speciale per gli utenti anonimi, dove "anonymous" è definito come un utente in cui il *IIdentity* restituisce proprietà *false*. Gli scenari includono fornendo la protezione XSRF alla pagina di accesso (prima che l'utente viene autenticato) e gli schemi di autenticazione personalizzata in cui l'applicazione utilizza un meccanismo diverso da *IIdentity* per identificare gli utenti.

Per supportare questi scenari, tenere presente che i token di sessione e campo vengono uniti mediante un token di sicurezza, è un identificatore opaco 128 bit generato casualmente. Questo token di sicurezza viene utilizzato per tenere traccia di una sessione utente passa il sito, in quanto costituisce effettivamente lo scopo di un identificatore anonimo. Una stringa vuota viene utilizzata al posto del nome utente per le routine di convalida e generazione descritte in precedenza.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / basata sulle attestazioni autenticazione

In genere, il *IIdentity* classi integrate in .NET Framework dispongono di proprietà che *IIdentity.Name* è sufficiente per identificare in modo univoco un utente specifico all'interno di una determinata applicazione. Ad esempio, *FormsIdentity.Name* restituisce il nome utente archiviato nel database delle appartenenze (che è univoco per tutte le applicazioni a seconda che il database), *WindowsIdentity.Name* restituisce il identità di dominio completo dell'utente e così via. Questi sistemi forniscono non solo l'autenticazione. sono inoltre *identificare* agli utenti di un'applicazione.

Autenticazione basata sulle attestazioni, d'altra parte, non richiede necessariamente che identifica un determinato utente. Al contrario, il *ClaimsPrincipal* e *ClaimsIdentity* tipi sono associati a un set di *attestazione* istanze, in cui le attestazioni singoli potrebbero essere "è 18 anni di età" o " è un amministratore"in modo diverso. Poiché l'utente non è necessariamente identificata, il runtime non è possibile usare il *ClaimsIdentity.Name* proprietà come un identificatore univoco per questo utente specifico. Il team ha riscontrato esempi reali in cui *ClaimsIdentity.Name* restituisce *null*, restituisce un nome descrittivo (visualizzazione) o in caso contrario, restituisce una stringa che non è appropriata per l'utilizzo come identificatore univoco per l'utente.

Molte delle distribuzioni che usano l'autenticazione basata sulle attestazioni utilizza [Azure Access Control Service](https://msdn.microsoft.com/en-us/library/windowsazure/gg429786.aspx) (ACS) in particolare. ACS consente allo sviluppatore di configurare singoli *provider di identità* (ad esempio ADFS, il provider dell'Account Microsoft, provider OpenID ad esempio Yahoo!, e così via), e restituiscono i provider di identità *nome identificatori*. Questi identificatori del nome possono contenere informazioni identificabili personalmente (PII) simile a un indirizzo di posta elettronica o potrebbe essere resi anonimi come un ID personale privato (PPID). Indipendentemente da ciò, la tupla (provider di identità, identificatore del nome) sufficientemente funge da un token di rilevamento appropriato per un determinato utente mentre l'utente è esplorando il sito, il Runtime di ASP.NET Web Stack consentono di usare la tupla al posto del nome utente durante la generazione e convalida i token di campo anti-XSRF. Gli URI per il provider di identità e l'identificatore del nome particolare sono:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(vedere questo [pagina doc ACS](https://msdn.microsoft.com/en-us/library/windowsazure/gg185971.aspx) per altre informazioni.)

Durante la generazione o la convalida di un token, il Runtime di ASP.NET Web Stack tenterà in fase di esecuzione dell'associazione per i tipi:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35`(Per il SDK WIF.)
- `System.Security.Claims.ClaimsIdentity`(Per .NET 4.5).

Se questi tipi esistono e se l'utente corrente *IIIIdentity* implementa o sottoclassi uno di questi tipi, si utilizzerà la funzionalità di anti-XSRF (provider di identità, identificatore del nome) tupla al posto del nome utente durante la generazione e convalida i token. Se tale tupla non è presente, la richiesta avrà esito negativo con un errore che descrive allo sviluppatore di come configurare il sistema di anti-XSRF per comprendere il meccanismo di autenticazione basata sulle attestazioni specifico in uso. Vedere il  **[configurazione e l'estensibilità](#_Configuration_and_extensibility)**  sezione per ulteriori informazioni.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID autenticazione

Infine, la funzionalità di anti-XSRF ha un supporto speciale per le applicazioni che usano l'autenticazione OAuth o OpenID. Questo supporto è basato su euristica: se corrente *IIdentity.Name* inizi con http:// o https://, quindi verrà eseguito il confronto di nome utente utilizzando un confronto ordinale anziché l'operatore di confronto OrdinalIgnoreCase.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configurazione e l'estensibilità

In alcuni casi, gli sviluppatori potrebbero essere maggiore controllo sulla generazione di anti-XSRF e i comportamenti di convalida. Ad esempio, ad esempio il comportamento predefinito helper MVC e pagine Web di aggiungere automaticamente i cookie HTTP alla risposta indesiderato, e lo sviluppatore potrebbe voler mantenere i token in un' posizione. Esistono due API per agevolare questo:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

Il *GetTokens* metodo accetta come input un XSRF richiesta verifica sessione token esistente (che può essere null) e produce come output di un nuovo token della sessione verifica XSRF richiesta e un token di campo. I token sono stringhe opache semplicemente senza decorazioni; il *formToken* valore sarà ad esempio non essere eseguito il wrapping in un &lt;input&gt; tag. Il *newCookieToken* valore può essere null; in questo caso, il *oldCookieToken* valore è ancora valido e non deve essere impostata alcun nuovo cookie di risposta. Il chiamante di *GetTokens* è responsabile della persistenza di cookie di risposta necessarie o la generazione di alcun markup necessarie; il *GetTokens* metodo stesso non modificherà la risposta come un effetto collaterale. Il *convalida* metodo accetta una sessione in ingresso e i token di campo e viene eseguita la logica di convalida menzionati in precedenza su di essi.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Lo sviluppatore può configurare il sistema di anti-XSRF dal applicazione\_Start. La configurazione è a livello di codice. Le proprietà di statica *AntiForgeryConfig* tipo sono descritti di seguito. La maggior parte degli utenti che utilizzano attestazioni saranno necessario impostare la proprietà UniqueClaimTypeIdentifier.

| **Property** | **Descrizione** |
| --- | --- |
| **AdditionalDataProvider** | Un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) che fornisce dati aggiuntivi durante la generazione del token e utilizza i dati aggiuntivi durante la convalida del token. Il valore predefinito è *null*. Per ulteriori informazioni, vedere il [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) sezione. |
| **CookieName** | Stringa che specifica il nome del cookie HTTP che viene utilizzato per memorizzare il token anti-XSRF della sessione. Se questo valore non è impostato, verrà generato un nome da automaticamente in base al percorso virtuale distribuito dell'applicazione. Il valore predefinito è *null*. |
| **RequireSsl** | Valore booleano che determina se il token anti-XSRF sono necessarie da inviare tramite un canale protetto con SSL. Se questo valore è *true*, i cookie generati automaticamente verranno impostati il flag "secure" e le API di anti-XSRF genererà se chiamato dall'interno di una richiesta che non viene inviata tramite SSL. Il valore predefinito è *false*. |
| **SuppressIdentityHeuristicChecks** | Valore booleano che determina se il sistema di anti-XSRF verrà disattivato il supporto per le identità basata sulle attestazioni. Se questo valore è *true*, presuppone che il sistema *IIdentity.Name* è appropriato per l'utilizzo come identificatore univoco per ogni utente e non tenterà di casi speciali *IClaimsIdentity*o *ClClaimsIdentity* come descritto nel [WIF / ACS / basata sulle attestazioni autenticazione](#_WIF_ACS) sezione. Il valore predefinito è `false`. |
| **UniqueClaimTypeIdentifier** | Una stringa che indica quale tipo di attestazione di tipo è appropriata per l'utilizzo come identificatore univoco per ogni utente. Se questo valore viene impostato e corrente *IIdentity* basata sulle attestazioni, il sistema verrà eseguito un tentativo di estrarre un'attestazione del tipo specificata da *UniqueClaimTypeIdentifier*, e verrà utilizzato il valore corrispondente al posto di nome utente dell'utente durante la generazione di token di campo. Se non viene trovato il tipo di attestazione, il sistema verrà eseguito la richiesta. Il valore predefinito è *null*, che indica che il sistema deve usare (provider di identità, identificatore del nome) tupla, come descritto in precedenza al posto di nome utente dell'utente. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

Il  *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)*  tipo consente agli sviluppatori di estendere il comportamento del sistema anti-XSRF, dati aggiuntivi di andata e ritorno in ogni token. Il *GetAdditionalData* metodo viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato. Un responsabile dell'implementazione può restituire un timestamp, un parametro nonce o qualsiasi altro valore che lei desidera da questo metodo.

Analogamente, il *ValidateAdditionalData* metodo viene chiamato ogni volta che viene convalidato un token di campo e la stringa "dati aggiuntivi" che è stata incorporata all'interno del token viene passata al metodo. La routine di convalida è possibile implementare un timeout (controllando l'ora corrente con l'ora in cui è stato archiviato quando è stato creato il token), un parametro nonce controllo routine o qualsiasi altra desiderata logica.

## <a name="design-decisions-and-security-considerations"></a>Decisioni di progettazione e considerazioni sulla sicurezza

Il token di sicurezza che collega i token di sessione e il campo è tecnicamente necessario solo quando si tenta di proteggere gli utenti anonimi / non autenticati da attacchi XSRF. Quando l'utente è autenticato, il token di autenticazione stesso (presumibilmente presentate sotto forma di un cookie) potrebbe essere utilizzato come una coppia di token metà di un programma di sincronizzazione. Tuttavia, esistono scenari validi per la protezione delle pagine di accesso per gli utenti non autenticati di passaggi e la logica di anti-XSRF è stata resa più semplice da sempre la generazione e convalida del token di sicurezza, anche per gli utenti autenticati. È inoltre fornire un livello di protezione aggiuntiva nel caso in cui un token di campo mai viene compromesso da un utente malintenzionato, come l'impostazione o un'ipotesi che il token di sessione sarebbe un altro ostacolo per l'utente malintenzionato che consentono di superare.

Gli sviluppatori devono usare cautela quando più applicazioni sono ospitate in un singolo dominio. Ad esempio, anche se *example1.cloudapp.net* e *example2.cloudapp.net* sono host diversi, c'è una relazione di trust implicita tra tutti gli host sotto il  *\*. cloudapp.net* dominio. Questa relazione di trust implicita [consente agli host potenzialmente non attendibili a hanno effetto sui cookie reciproci](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (lo stessa origine criteri che determinano le richieste AJAX non sono necessariamente applicabili per i cookie HTTP). Il Runtime di ASP.NET Web Stack fornisce alcuni mitigazione in quanto il nome utente è incorporato nel token di campo, pertanto anche se un sottodominio dannoso è in grado di sovrascrivere un token di sessione non sarà in grado di generare un token di campo valido per l'utente. Tuttavia, quando sono ospitati in tale ambiente le routine di anti-XSRF incorporato ancora non è possibile difendersi Hijack della sessione o l'account di accesso XSRF.

Le routine di anti-XSRF attualmente non difendersi [clickjacking](https://www.owasp.org/index.php/Clickjacking). Alle applicazioni per difendersi clickjacking possono farlo facilmente inviando un X-Frame-Options: intestazione SAMEORIGIN con ogni risposta. Questa intestazione è supportata da tutti i browser recenti. Per ulteriori informazioni, vedere il [blog di inserimento/espulsione](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), e [OWASP](https://www.owasp.org/index.php/Clickjacking). Il Runtime di ASP.NET Web Stack può in alcuni verificare versione futura di MVC e gli helper di anti-XSRF pagine Web questa intestazione viene impostata automaticamente in modo che le applicazioni siano automaticamente protetti da questo tipo di attacco.

Gli sviluppatori Web devono continuare ad assicurarsi che il sito non sia vulnerabile agli attacchi XSS. Se l'attacco riesce anche interromperebbe il Runtime di ASP.NET Web Stack difese da attacchi XSRF attacchi XSS sono molto potenti.

## <a name="acknowledgment"></a>Riconoscimento

[@LeviBroderick](https://twitter.com/LeviBroderick), che ha scritto la maggior parte del codice della sicurezza ASP.NET la maggior parte di queste informazioni.
