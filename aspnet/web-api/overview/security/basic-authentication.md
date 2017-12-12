---
uid: web-api/overview/security/basic-authentication
title: Autenticazione di base in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: Viene descritto l'utilizzo di autenticazione di base nell'API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="basic-authentication-in-aspnet-web-api"></a>Autenticazione di base in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

L'autenticazione di base è definito in [RFC 2617, HTTP Authentication: Basic and Digest Authentication accesso](http://www.ietf.org/rfc/rfc2617.txt).

Svantaggi

- Credenziali utente vengono inviate nella richiesta.
- Le credenziali vengono inviate come testo non crittografato.
- Le credenziali vengono inviate con ogni richiesta.
- Non è possibile effettuare la disconnessione, tranne che terminando la sessione del browser.
- Vulnerabile ad tra siti richiesta forgery (CSRF); richiede le misure di anti-CSRF.

Vantaggi

- Standard di Internet.
- Supportato da tutti i browser principali.
- Protocollo relativamente semplice.

L'autenticazione di base funziona nel modo seguente:

1. Se una richiesta richiede l'autenticazione, il server restituisce 401 (non autorizzato). La risposta include un'intestazione WWW-Authenticate, che indica che il server supporta l'autenticazione di base.
2. Il client invia un'altra richiesta, con le credenziali del client nell'intestazione di autorizzazione. Le credenziali vengono formattate come stringa "name: password", con codifica base64. Le credenziali non vengono crittografate.

L'autenticazione di base viene eseguita nel contesto di un' "area di autenticazione." Il server include il nome dell'area di autenticazione nell'intestazione WWW-Authenticate. Le credenziali dell'utente sono valide all'interno di tale area di autenticazione. L'ambito esatto di un'area di autenticazione è definito dal server. Ad esempio, è possibile definire diverse aree di autenticazione in ordine alle risorse di partizione.

![](basic-authentication/_static/image1.png)

Poiché le credenziali vengono inviate non crittografate, l'autenticazione di base è protetto solo tramite HTTPS. Vedere [Working with SSL in Web API](working-with-ssl-in-web-api.md).

L'autenticazione di base è vulnerabile agli attacchi CSRF. Dopo che l'utente immette le credenziali, il browser automaticamente inviate nelle richieste successive allo stesso dominio, per la durata della sessione. Sono incluse le richieste AJAX. Vedere [impedire attacchi di tipo Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticazione di base con IIS

IIS supporta l'autenticazione di base, ma è sempre così: l'utente è autenticato presso le proprie credenziali di Windows. Ciò significa che l'utente deve disporre di un account nel dominio del server. Per un sito web pubblico, si desidera in genere per autenticare un provider di appartenenze ASP.NET.

Per abilitare l'autenticazione di base utilizzando IIS, impostare la modalità di autenticazione per "Windows" in Web. config del progetto ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

In questa modalità, IIS Usa le credenziali di Windows per l'autenticazione. Inoltre, è necessario abilitare l'autenticazione di base in IIS. In Gestione IIS, passare alla visualizzazione funzionalità, selezionare l'autenticazione e abilitare l'autenticazione di base.

![](basic-authentication/_static/image2.png)

Nel progetto API Web, aggiungere il `[Authorize]` attributo per le azioni del controller che richiedono l'autenticazione.

Un client si autentica impostando l'intestazione di autorizzazione nella richiesta. I client del browser eseguono questa operazione automaticamente. I client non-browser saranno necessario impostare l'intestazione.

## <a name="basic-authentication-with-custom-membership"></a>Autenticazione di base con appartenenza personalizzata

Come accennato, l'autenticazione di base incorporate in IIS utilizza le credenziali di Windows. Pertanto, che è necessario creare account per gli utenti nel server di hosting. Tuttavia, per un'applicazione internet, gli account utente vengono in genere archiviati in un database esterno.

Nell'esempio di codice come un modulo HTTP che esegue l'autenticazione di base. È possibile collegare facilmente in un provider di appartenenze ASP.NET sostituendo il `CheckPassword` metodo, che è un metodo in questo esempio fittizio.

In Web API 2, è necessario considerare la scrittura un [filtro autenticazione](authentication-filters.md) o [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), invece di un modulo HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Per abilitare il modulo HTTP, aggiungere quanto segue al file Web. config nel **System. webServer** sezione:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Sostituire "NomeAssembly" con il nome dell'assembly (senza includere l'estensione "dll").

È consigliabile disabilitare altri schemi di autenticazione, ad esempio autenticazione di Windows o di form
