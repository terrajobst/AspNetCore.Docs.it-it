---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Utilizzo di SSL in Web API | Documenti Microsoft
author: MikeWasson
description: Viene illustrato come utilizzare SSL con l'API Web ASP.NET, incluso l'utilizzo di certificati client SSL.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="working-with-ssl-in-web-api"></a>Utilizzo di SSL in Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Diversi schemi di autenticazione comuni non sono protetti tramite HTTP semplice. In particolare, l'autenticazione di base e autenticazione basata su form invia le credenziali non crittografate. Per essere sicuro, questi schemi di autenticazione *deve* utilizzare SSL. Inoltre, è possono utilizzare i certificati client SSL per autenticare i client.

## <a name="enabling-ssl-on-the-server"></a>Abilitazione di SSL nel Server

Per configurare SSL in IIS 7 o versioni successive:

- Creare o ottenere un certificato. Per i test, è possibile creare un certificato autofirmato.
- Aggiungere un binding HTTPS.

Per informazioni dettagliate, vedere [come impostazione di SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Per il test locale, è possibile attivare SSL in IIS Express di Visual Studio. Nella finestra Proprietà impostare **SSL abilitato** a **True**. Prendere nota del valore di **URL SSL**; usare questo URL per il test delle connessioni HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Applicazione di SSL in un Controller API Web

Se si dispone di un'associazione HTTP e HTTPS, i client possono ancora utilizzare HTTP per accedere al sito. È possibile consentire alcune risorse necessarie per rendere disponibile tramite HTTP, mentre altre risorse richiedono SSL. In tal caso, è possibile utilizzare un filtro azione per richiedere SSL per le risorse protette. Il codice seguente viene illustrato un filtro di autenticazione API Web che verifica la presenza di SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Aggiungere il filtro per le azioni di API Web che richiedono SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificati Client SSL

SSL fornisce l'autenticazione tramite certificati di infrastruttura a chiave pubblica. Il server deve fornire un certificato che autentica il server al client. È meno comune per il client di fornire un certificato al server, ma si tratta di un'opzione per l'autenticazione dei client. Per utilizzare i certificati client con SSL, è necessario un modo per distribuire i certificati agli utenti. Per molti tipi di applicazione, non sarà un'esperienza utente soddisfacente, ma in alcuni ambienti (ad esempio, enterprise) può risultare appropriato.

| Vantaggi | Svantaggi |
| --- | --- |
| Le credenziali del certificato sono più avanzate rispetto a nome utente/password. -SSL fornisce un canale sicuro completo, con l'autenticazione, l'integrità del messaggio e la crittografia dei messaggi. | -È necessario ottenere e gestire i certificati di infrastruttura a chiave pubblica. -La piattaforma del client deve supportare i certificati client SSL. |

Per configurare IIS per accettare i certificati client, aprire Gestione IIS ed eseguire i passaggi seguenti:

1. Fare clic sul nodo di sito nella visualizzazione albero.
2. Fare doppio clic su di **impostazioni SSL** funzionalità nel riquadro centrale.
3. In **i certificati Client**, selezionare una delle seguenti opzioni: 

    - **Accettare**: IIS accetterà un certificato dal client, ma non ne richiede.
    - **Richiedi**: richiede un certificato client. (Per abilitare questa opzione, è necessario selezionare anche "Richiedi SSL")

È inoltre possibile impostare queste opzioni nel file ApplicationHost. config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

Il **SslNegotiateCert** flag significa IIS accetterà un certificato dal client, ma non ne richiede (equivalente all'opzione "Accetta" in Gestione IIS). Per richiedere un certificato, impostare il **SslRequireCert** flag. Per i test, è anche possibile impostare queste opzioni in IIS Express, il applicationhost locale. File di configurazione, che si trova in "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Creazione di un certificato Client per il test

A scopo di test, è possibile utilizzare [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) per creare un certificato client. Creare innanzitutto un'autorità radice di test:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert verrà richiesto di immettere una password per la chiave privata.

Successivamente, aggiungere il certificato per il test di archiviazione "Autorità di certificazione" del server, come indicato di seguito:

1. Aprire MMC.
2. In **File**selezionare **Aggiungi/Rimuovi Snap-In**.
3. Selezionare **Account Computer**.
4. Selezionare **computer locale** e completare la procedura guidata.
5. Nel riquadro di spostamento, espandere il nodo "Autorità di certificazione".
6. Nel **azione** dal menu **tutte le attività**, quindi fare clic su **importazione** per avviare l'importazione guidata certificati.
7. Individuare il file di certificato, TempCA.cer.
8. Fare clic su **aprire**, quindi fare clic su **Avanti** e completare la procedura guidata. (Verrà richiesto di immettere nuovamente la password.)

Creare ora un certificato client sia firmata dal primo certificato:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Utilizzo dei certificati Client in Web API

Sul lato server, è possibile ottenere il certificato client chiamando [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) nel messaggio di richiesta. Il metodo restituisce null se nessun certificato client. In caso contrario, restituisce un **X509Certificate2** istanza. Utilizzare questo oggetto per ottenere informazioni sul certificato, ad esempio l'autorità emittente e l'oggetto. È quindi possibile utilizzare queste informazioni per l'autenticazione e/o di autorizzazione.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
