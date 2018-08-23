---
uid: web-api/overview/security/basic-authentication
title: L'autenticazione di base nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo dell'autenticazione di base nell'API Web ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7afafb6b7851f0d955d1f4292318f64d2a068a45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838424"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Autenticazione di base nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

L'autenticazione di base è definito in [RFC 2617, autenticazione HTTP: Basic and Digest Authentication accesso](http://www.ietf.org/rfc/rfc2617.txt).

Svantaggi

- Le credenziali dell'utente vengono inviate nella richiesta.
- Le credenziali vengono inviate come testo non crittografato.
- Le credenziali vengono inviate con ogni richiesta.
- Nessun modo per disconnettersi, a meno che non termina la sessione del browser.
- Vulnerabile a XSS richiesta intersito falsa (CSRF); richiede misure Intersito.

Vantaggi

- Standard di Internet.
- Supportato da tutti i principali browser.
- Protocollo relativamente semplice.

L'autenticazione di base funziona nel modo seguente:

1. Se una richiesta richiede l'autenticazione, il server restituisce 401 (non autorizzato). La risposta include un'intestazione WWW-Authenticate, che indica che il server supporta l'autenticazione di base.
2. Il client invia un'altra richiesta, con le credenziali client nell'intestazione dell'autorizzazione. Le credenziali vengono formattate come stringa "name: password", con codifica base64. Le credenziali non vengono crittografate.

L'autenticazione di base viene eseguita nel contesto di un' "area di autenticazione." Il server include il nome dell'area di autenticazione nell'intestazione WWW-Authenticate. Le credenziali dell'utente sono valide all'interno di tale area di autenticazione. L'ambito esatto di un'area di autenticazione è definito dal server. Ad esempio, è possibile definire diverse aree di autenticazione nell'ordine alle risorse di partizione.

![](basic-authentication/_static/image1.png)

Perché le credenziali vengono inviate non crittografate, l'autenticazione di base è protetta solo tramite HTTPS. Visualizzare [Working with SSL in Web API](working-with-ssl-in-web-api.md).

Inoltre, l'autenticazione di base è vulnerabile agli attacchi CSRF. Dopo che l'utente immette le credenziali, il browser invia automaticamente tali nelle richieste successive allo stesso dominio, per la durata della sessione. Ciò include le richieste AJAX. Visualizzare [prevenzione degli attacchi di richiesta intersito falsa (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticazione di base con IIS

IIS supporta l'autenticazione di base, ma è presente una sola avvertenza: l'utente viene autenticato presso le proprie credenziali di Windows. Ciò significa che l'utente deve avere un account nel dominio del server. Per un sito web rivolte al pubblico, in genere consigliabile eseguire l'autenticazione in un provider di appartenenze ASP.NET.

Per abilitare l'autenticazione di base tramite IIS, impostare la modalità di autenticazione su "Windows" in Web. config del progetto ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

In questa modalità, IIS Usa le credenziali di Windows per eseguire l'autenticazione. Inoltre, è necessario abilitare l'autenticazione di base in IIS. In Gestione IIS, Vai a visualizzazione funzionalità, selezionare l'autenticazione e abilitare l'autenticazione di base.

![](basic-authentication/_static/image2.png)

Nel progetto API Web, aggiungere il `[Authorize]` attributo per eventuali azioni del controller che richiedono l'autenticazione.

Un client si autentica impostando l'intestazione di autorizzazione nella richiesta. I client del browser di eseguire automaticamente questo passaggio. I client non-browser saranno necessario impostare l'intestazione.

## <a name="basic-authentication-with-custom-membership"></a>Autenticazione di base con appartenenze personalizzato

Come accennato, l'autenticazione di base compilato in IIS utilizza le credenziali di Windows. Pertanto, che è necessario creare account per gli utenti nel server di hosting. Ma per un'applicazione internet, gli account utente vengono in genere archiviati in un database esterno.

Nell'esempio di codice come un modulo HTTP che esegue l'autenticazione di base. È possibile collegare facilmente in un provider di appartenenze ASP.NET, sostituendo il `CheckPassword` metodo, che è un metodo in questo esempio fittizio.

Nell'API Web 2, è necessario considerare la scrittura un' [filtro di autenticazione](authentication-filters.md) oppure [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), invece di un modulo HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Per abilitare il modulo HTTP, aggiungere quanto segue al file Web. config nel **System. webServer** sezione:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Sostituire "NomeAssembly" con il nome dell'assembly (senza includere l'estensione "dll").

È consigliabile disabilitare gli altri schemi di autenticazione, ad esempio autenticazione form o Windows.
