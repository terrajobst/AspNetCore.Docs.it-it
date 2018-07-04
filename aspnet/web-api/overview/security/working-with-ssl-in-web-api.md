---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Uso di SSL in API Web | Microsoft Docs
author: MikeWasson
description: Viene illustrato come usare SSL con l'API Web ASP.NET, incluso l'uso di certificati client SSL.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4e14607f11fcd376b4ceca4bf7862259abe015e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396630"
---
<a name="working-with-ssl-in-web-api"></a>Uso di SSL in API Web
====================
da [Mike Wasson](https://github.com/MikeWasson)

Numerosi schemi di autenticazione comuni non sono sicuri sul protocollo HTTP normale. In particolare, l'autenticazione di base e autenticazione basata su form inviano credenziali non crittografate. Per essere sicuri, questi schemi di autenticazione *necessario* utilizzare SSL. Inoltre, i certificati client SSL utilizzabile per autenticare i client.

## <a name="enabling-ssl-on-the-server"></a>Abilitazione di SSL nel Server

Per configurare SSL in IIS 7 o versioni successive:

- Creare o ottenere un certificato. Per i test, è possibile creare un certificato autofirmato.
- Aggiungere un binding HTTPS.

Per informazioni dettagliate, vedere [come impostazione di SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Per il test locale, è possibile abilitare SSL in IIS Express da Visual Studio. Nella finestra Proprietà impostare **SSL abilitato** al **True**. Prendere nota del valore di **URL SSL**; usare questo URL per testare le connessioni HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Imporre SSL in un Controller API Web

Se hai un HTTPS e un binding HTTP, i client possono comunque usare HTTP per accedere al sito. È possibile consentire alcune risorse necessarie per essere disponibile tramite HTTP, mentre altre risorse richiedono SSL. In tal caso, usare un filtro azione per richiedere SSL per le risorse protette. Il codice seguente mostra un filtro di autenticazione di API Web che verifica la presenza di SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Aggiungere il filtro selezionato per eventuali azioni di API Web che richiedono SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificati Client SSL

SSL consente l'autenticazione tramite certificati di infrastruttura a chiave pubblica. Il server deve fornire un certificato che autentica il server al client. È meno comune per il client di fornire un certificato al server, ma si tratta di un'opzione per l'autenticazione dei client. Per usare i certificati client con SSL, è necessario un modo per distribuire i certificati firmati agli utenti. Per molti tipi di applicazioni, questo non costituisce una buona esperienza utente, ma in alcuni ambienti (ad esempio, aziendale) può risultare appropriato.

| Vantaggi | Svantaggi |
| --- | --- |
| -Credenziali certificate sono più avanzate rispetto a nome utente/password. -SSL fornisce un canale sicuro completo, con l'autenticazione, l'integrità del messaggio e la crittografia dei messaggi. | -È necessario ottenere e gestire i certificati PKI. -La piattaforma client deve supportare i certificati client SSL. |

Per configurare IIS per accettare certificati client, aprire Gestione IIS e procedere come segue:

1. Fare clic sul nodo del sito nella visualizzazione albero.
2. Fare doppio clic il **impostazioni SSL** funzionalità nel riquadro centrale.
3. Sotto **certificati Client**, selezionare una delle opzioni seguenti: 

    - **Accettare**: IIS accetterà un certificato dal client, ma non ne richiede.
    - **Richiedi**: è necessario un certificato client. (Per abilitare questa opzione, è necessario selezionare anche "Richiedi SSL")

È anche possibile impostare queste opzioni nel file ApplicationHost. config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

Il **SslNegotiateCert** flag significa IIS accetterà un certificato dal client, ma non ne richiede (equivalente all'opzione "Accept" in Gestione IIS). Per richiedere un certificato, impostare il **SslRequireCert** flag. Per i test, è anche possibile impostare queste opzioni in IIS Express, il applicationhost locale. File di configurazione che si trova in "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Creazione di un certificato Client per i test

A scopo di test, è possibile usare [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) per creare un certificato client. Creare innanzitutto un'autorità radice di test:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert verrà chiesto di immettere una password per la chiave privata.

Successivamente, aggiungere il certificato per il test del server "Trusted Root archivio Autorità di certificazione", come indicato di seguito:

1. Aprire MMC.
2. Sotto **File**, selezionare **Aggiungi/Rimuovi Snap-In**.
3. Selezionare **Account del Computer**.
4. Selezionare **computer locale** e completare la procedura guidata.
5. Nel riquadro di spostamento, espandere il nodo "Radice autorità di certificazione".
6. Nel **azione** dal menu **tutte le attività**, quindi fare clic su **importazione** per avviare l'importazione guidata certificati.
7. Individuare il file di certificato, Tempca.
8. Fare clic su **aperto**, quindi fare clic su **successivo** e completare la procedura guidata. (Verrà richiesto di immettere nuovamente la password.)

A questo punto creare un certificato client sia firmata dal primo certificato:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Utilizzo dei certificati Client in API Web

Sul lato server, è possibile ottenere il certificato client chiamando [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) nel messaggio di richiesta. Il metodo restituisce null se nessun certificato client. In caso contrario, restituisce un **X509Certificate2** istanza. Usare questo oggetto per ottenere informazioni dal certificato, ad esempio l'autorità emittente e il soggetto. È quindi possibile usare queste informazioni per l'autenticazione e/o autorizzazione.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
