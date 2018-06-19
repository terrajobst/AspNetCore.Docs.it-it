---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Configurazione di un Server Web per il Web distribuire pubblicazione (agente remoto) | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come configurare un server web Internet Information Services (IIS) per supportare la distribuzione utilizzando la distribuzione Web di IIS e pubblicazione sul web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 9f3a55c5e68e61a2d7907c765209d3786e05a485
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2018
ms.locfileid: "34473207"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Configurazione di un Server Web per la pubblicazione (agente remoto) di distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare un server web Internet Information Services (IIS) per supportare la pubblicazione sul web e distribuzione tramite il servizio agente remoto di strumento di distribuzione Web di IIS (distribuzione Web).
> 
> Quando si lavora con distribuzione Web 2.0 o versione successiva, sono disponibili tre approcci principali, che è possibile utilizzare per ottenere le applicazioni o siti su un server web. È possibile:
> 
> - Utilizzare il *servizio remoto dell'agente di distribuzione Web*. Questo approccio richiede la configurazione del server web, ma è necessario fornire le credenziali dell'amministratore del server locale per poter distribuire qualsiasi elemento nel server.
> - Utilizzare il *gestore distribuzione Web*. Questo approccio è molto più complesso e richiede uno sforzo iniziale per configurare il server web. Tuttavia, quando si utilizza questo approccio, è possibile configurare IIS per consentire agli utenti senza privilegi di amministratore eseguire la distribuzione. Il gestore di distribuzione Web è disponibile solo in IIS 7 o versione successiva.
> - Utilizzare *distribuzione non in linea*. Questo approccio richiede la minima configurazione del server web, ma un amministratore del server deve copiare il pacchetto web sul server e importarlo tramite Gestione IIS manualmente.
> 
> Per ulteriori informazioni sulla funzionalità chiave, vantaggi e svantaggi di questi approcci, vedere [scelta dell'approccio di destra per distribuzione Web](choosing-the-right-approach-to-web-deployment.md).


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>È Web distribuire l'agente remoto, l'approccio giusto per l'utente?

Sì, se l'utente che verrà distribuito il contenuto può fornire le credenziali di amministratore nel server di destinazione. Questo approccio risulta spesso utile in questi tipi di scenari:

- Ambienti sviluppo o test, in cui lo sviluppatore ha il pieno controllo sul server web di destinazione e il server di database.
- Organizzazioni più piccole in cui un singolo utente o un piccolo gruppo di utenti ha controllo sul ciclo di vita intera applicazione.

In un numero elevato di organizzazioni più grandi e in particolare per gli ambienti di produzione o gestione temporanea, è spesso non realistico per concedere agli utenti diritti di amministratore nei server web. Nel caso dei server di hosting web, questo è particolarmente poco sia il caso. Inoltre, se si prevede di automatizzare la distribuzione da un server di compilazione, potrebbe non desidera utilizzare le credenziali di amministratore per il processo di distribuzione. In questi scenari, la configurazione dei server web per supportare la distribuzione utilizzando il [gestore distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) può fornire una scelta più soddisfacente.

## <a name="task-overview"></a>Panoramica di Task

In questo argomento viene descritto come configurare un server web Internet Information Services (IIS) 7.5 per accettare e distribuire pacchetti web da un computer remoto utilizzando l'approccio di agente remoto di distribuzione Web. È necessario:

- Installare IIS 7.5 e la configurazione consigliata di IIS 7.
- Installare distribuzione Web 2.1 o versione successiva.
- Creare un sito Web IIS per ospitare il contenuto distribuito.
- Verificare che il servizio agente di distribuzione Web è in esecuzione.

Per ospitare la soluzione di esempio, in particolare, è anche necessario:

- Installare .NET Framework 4.0.
- Installare ASP.NET MVC 3.

Questo argomento viene illustrato come eseguire ognuna di queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono che si sta iniziando con una compilazione pulita server che esegue Windows Server 2008 R2. Prima di continuare, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili vengono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e registrazione](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Il servizio agente remoto è supportato a partire da IIS 6 e non richiede di essere aggiunto a un dominio. Tuttavia, i passaggi descritti in questa esercitazione sono stati sviluppati e testati in IIS 7.5 e procedure per le altre versioni possono variare.


## <a name="install-products-and-components"></a>Installare i prodotti e componenti

In questa sezione vi assisterà durante l'installazione di componenti e i prodotti necessari nel server web. Prima di iniziare, una procedura consigliata consiste nell'eseguire Windows Update per verificare che il server è completamente aggiornato.

In questo caso, è necessario installare quanto segue:

- **Configurazione consigliata per IIS 7**. In questo modo il **Server Web (IIS)** ruolo sul server web e installa il set di moduli IIS e i componenti necessari per ospitare un'applicazione ASP.NET.
- **.NET Framework 4.0**. Ciò è necessario per eseguire applicazioni che sono state compilate in questa versione di .NET Framework.
- **2.1 o successivo dello strumento di distribuzione Web**. Questo modo vengono installati sul server di distribuzione Web (e il relativo file eseguibile sottostante, MSDeploy.exe). Come parte di questo processo, installa e avvia il servizio agente di distribuzione Web. Questo servizio consente di distribuire i pacchetti web da un computer remoto.
- **ASP.NET MVC 3**. Consente di installare gli assembly che necessari per eseguire applicazioni MVC 3.

> [!NOTE]
> Questa procedura dettagliata viene descritto l'utilizzo dell'installazione guidata piattaforma Web per installare e configurare i componenti necessari. Anche se non è necessario utilizzare l'installazione guidata piattaforma Web, semplifica il processo di installazione automaticamente il rilevamento delle dipendenze e verificando che si ottengano sempre le versioni del prodotto più recenti. Per ulteriori informazioni, vedere [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/?linkid=9805118).


**Per installare i componenti e prodotti necessari**

1. Scaricare e installare il [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).
2. Quando l'installazione è stata completata, l'installazione guidata piattaforma Web verrà avviato automaticamente.

    > [!NOTE]
    > È ora possibile avviare l'installazione guidata piattaforma Web in qualsiasi momento dal **avviare** menu. A tale scopo, scegliere il **avviare** menu, fare clic su **tutti i programmi**e quindi fare clic su **installazione guidata piattaforma Web di Microsoft**.
3. Nella parte superiore del **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
4. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Framework**.
5. Nel **Microsoft .NET Framework 4** riga se .NET Framework non è già installato, fare clic su **Aggiungi**.

    > [!NOTE]
    > Si è già installato .NET Framework 4.0 in Windows Update. Se è già installato un prodotto o componente, l'installazione guidata piattaforma Web indicherà questo sostituendo il **Aggiungi** pulsante con il testo **installato**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. Nel **ASP.NET MVC 3 (Visual Studio 2010)** di riga, fare clic su **Aggiungi**.
7. Nel riquadro di spostamento, fare clic su **Server**.
8. Nel **configurazione consigliata di IIS 7** di riga, fare clic su **Aggiungi**.
9. Nel **2.1 dello strumento di distribuzione Web** di riga, fare clic su **Aggiungi**.
10. Fare clic su **Installa**. L'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;con le dipendenze associate&#x2014;sia installato e verrà richiesto di accettare le condizioni di licenza.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Esaminare le condizioni di licenza e, se accettano le condizioni, fare clic su **accetto**.
12. Una volta completato l'installazione, fare clic su **fine**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

Se è installato .NET Framework 4.0 prima di installare IIS, è necessario eseguire il [strumento di registrazione ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) per registrare la versione più recente di ASP.NET con IIS. Se non eseguire questa operazione, sono disponibili che IIS verrà contenuto statico (ad esempio file HTML) senza problemi, ma restituirà **404.0 Errore HTTP: non trovato** quando si tenta di passare al contenuto ASP.NET. È possibile utilizzare questa procedura per assicurarsi che sia registrato in ASP.NET 4.0.

**Per registrare ASP.NET 4.0 con IIS**

1. Fare clic su **avviare**, quindi digitare **prompt dei comandi**.
2. Nei risultati della ricerca, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.
3. Nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Se si prevede di ospitare applicazioni web a 64 bit in qualsiasi momento, è inoltre necessario registrare la versione a 64 bit di ASP.NET con IIS. A tale scopo, nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

È buona norma, utilizzare Windows Update nuovamente a questo punto per scaricare e installare eventuali aggiornamenti disponibili per i nuovi prodotti e componenti di cui che è installato.

## <a name="configure-the-iis-website"></a>Configurare il sito Web IIS

Prima di poter distribuire il contenuto web al server, è necessario creare e configurare un sito Web IIS per ospitare il contenuto. Distribuzione Web è possibile distribuire solo pacchetti web a un sito Web IIS esistente; Impossibile creare il sito Web. In generale, è necessario completare queste attività:

- Creare una cartella nel file system per ospitare il contenuto.
- Creare un sito Web IIS per fornire il contenuto e associarlo a una cartella locale.
- Concedere autorizzazioni di lettura per l'identità del pool di applicazioni per la cartella locale.

Anche se non c'è niente di arresto dalla distribuzione del contenuto per il sito Web predefinito in IIS, questo approccio non è consigliato per qualsiasi elemento diverso da scenari di test o dimostrazione. Per simulare un ambiente di produzione, è necessario creare un nuovo sito Web IIS con le impostazioni specifiche per i requisiti dell'applicazione.

**Per creare e configurare un sito Web IIS**

1. Nel file system locale, creare una cartella per archiviare il contenuto (ad esempio, **C:\DemoSite**).
2. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)**.
3. In Gestione IIS, nel **connessioni** riquadro espandere il nodo del server (ad esempio, **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Fare doppio clic su di **siti** nodo e quindi fare clic su **Aggiungi sito Web**.
5. Nel **nome sito** , digitare un nome per il sito Web IIS (ad esempio, **DemoSite**).
6. Nel **percorso fisico** digitare (o passare a) il percorso di cartella locale (ad esempio, **C:\DemoSite**).
7. Nel **porta** , digitare il numero di porta su cui si desidera ospitare il sito Web (ad esempio, **85**).

    > [!NOTE]
    > I numeri di porta standard sono 80 per HTTP e 443 per HTTPS. Tuttavia, se si ospita il sito Web sulla porta 80, è necessario arrestare il sito Web predefinito prima di poter accedere al sito.
8. Lasciare il **nome Host** casella vuota, a meno che non si desidera configurare un record Domain Name System (DNS) per il sito Web e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > In un ambiente di produzione sarà sicuramente si desidera ospitare il sito Web sulla porta 80 e configurare un'intestazione host, con i record DNS corrispondenti. Per ulteriori informazioni sulla configurazione di IIS 7 intestazioni host, vedere [configurare un'intestazione Host per un sito Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Per ulteriori informazioni sul ruolo Server DNS in Windows Server 2008 R2, vedere [Panoramica del Server DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) e [Server DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Nel riquadro **Azioni** sotto **Modifica sito**, fare clic su **Binding**.
10. Nel **binding sito** la finestra di dialogo, fare clic su **Aggiungi**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. Nel **Aggiungi Binding sito** la finestra di dialogo, impostare il **indirizzo IP** e **porta** in modo che corrisponda alla configurazione del sito esistente.
12. Nel **nome Host** , digitare il nome del server web (ad esempio, **TESTWEB1**), quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > Consente l'associazione del sito prima di accedere al sito localmente utilizzando l'indirizzo IP e porta o `http://localhost:85`. L'associazione del sito secondo consente di accedere al sito da altri computer nel dominio utilizzando il nome del computer (ad esempio, http://testweb1:85).
13. Nel **binding sito** la finestra di dialogo, fare clic su **Chiudi**.
14. Nel **connessioni** riquadro, fare clic su **pool di applicazioni**.
15. Nel **pool di applicazioni** riquadro destro il nome del pool di applicazioni e quindi fare clic su **le impostazioni di base**. Per impostazione predefinita, il nome del pool di applicazioni corrisponderà al nome del sito Web (ad esempio, **DemoSite**).
16. Nel **versione di .NET Framework** elenco, selezionare **.NET Framework v 4.0.30319**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > La soluzione di esempio richiede .NET Framework 4.0. Questo non è un requisito per distribuzione Web in generale.

Affinché il sito Web fornire contenuto, l'identità del pool di applicazioni deve disporre delle autorizzazioni per la cartella locale che contiene il contenuto lettura. In IIS 7.5, pool di applicazioni eseguiti con un'identità di pool di applicazione univoco per impostazione predefinita (a differenza delle versioni precedenti di IIS, in cui i pool di applicazioni verrebbero in genere eseguito l'account del servizio di rete). L'identità del pool non è un account utente reale e non vengono visualizzati in qualsiasi elenco di utenti o gruppi&#x2014;viene invece creata in modo dinamico quando viene avviato il pool di applicazioni. Ogni identità di pool di applicazioni viene aggiunto all'oggetto locale **IIS\_IUSRS** gruppo di sicurezza come elemento nascosto.

Per concedere autorizzazioni a un'identità del pool di applicazioni in un file o una cartella, sono disponibili due opzioni:

- Assegnare autorizzazioni all'identità del pool di applicazioni direttamente, utilizzando il formato <strong>IIS AppPool\</ strong ><em>[nome pool di applicazioni]</em>(ad esempio, <strong>IIS AppPool\DemoSite</strong>).
- Assegnare autorizzazioni per il **IIS\_IUSRS** gruppo.

L'approccio più comune consiste nell'assegnare autorizzazioni a locale **IIS\_IUSRS** gruppo perché questo approccio consente di modificare i pool di applicazioni senza dover riconfigurare le autorizzazioni del file. La procedura successiva Usa questo approccio in base al gruppo.

> [!NOTE]
> Per ulteriori informazioni sull'identità del pool di applicazioni in IIS 7.5, vedere [le identità del Pool di applicazioni](https://go.microsoft.com/?linkid=9805123).


**Per configurare le autorizzazioni per un sito Web IIS**

1. In Esplora risorse, passare al percorso della cartella locale.
2. Fare clic sulla cartella e quindi fare clic su **proprietà**.
3. Nel **sicurezza** scheda, fare clic su **modifica**e quindi fare clic su **Aggiungi**.
4. Fare clic su **percorsi**. Nel **percorsi** la finestra di dialogo, selezionare il server locale, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. Nel **Seleziona utenti o gruppi** della finestra di dialogo tipo **IIS\_IUSRS**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.
6. Nel <strong>le autorizzazioni per</strong><em>[nome cartella]</em>la finestra di dialogo, si noti che il nuovo gruppo è stato assegnato il <strong>lettura &amp; eseguire</strong>, <strong>visualizzazione contenuto cartella contenuto</strong>, e <strong>lettura</strong> le autorizzazioni per impostazione predefinita. Lasciare invariata e fare clic su <strong>OK</strong>.
7. Fare clic su <strong>OK</strong> per chiudere la <em>[nome cartella]</em><strong>proprietà</strong> la finestra di dialogo.

Come attività finale prima di tentare di distribuire tutti i pacchetti web sul server, è necessario assicurarsi che il servizio agente di distribuzione Web è in esecuzione. Quando si distribuisce un pacchetto da un computer remoto, il servizio agente di distribuzione Web è responsabile per l'estrazione e il contenuto del pacchetto di installazione. Il servizio viene avviato per impostazione predefinita, quando si installa lo strumento di distribuzione Web e viene eseguito con l'identità del servizio di rete.

È possibile verificare se un servizio è in esecuzione in più modi diversi, utilizzando diverse utilità della riga di comando o i cmdlet di Windows PowerShell. Questa procedura viene descritto un approccio basato su interfaccia utente semplice.

**Per verificare che il servizio agente di distribuzione Web è in esecuzione**

1. Scegliere **Start** , **Strumenti di amministrazione**, quindi scegliere **Servizi**.
2. Individuare il **servizio agente distribuzione Web** di riga e verificare che il **stato** è impostato su **Started**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Se il servizio non è già avviato, fare clic su **avviare**.

## <a name="configure-firewall-exceptions"></a>Configurare le eccezioni del Firewall

Per impostazione predefinita, il servizio agente remoto è in ascolto sulla porta TCP 80, a questo URL:

<http://servername.com/MSDEPLOYAGENTSERVICE>

Nella maggior parte dei casi, non sarà necessario configurare tutte le regole aggiuntive del firewall per il servizio agente remoto, perché i server web in genere l'ascolto delle richieste HTTP sulla porta 80. Se si personalizza l'installazione per l'ascolto su una porta non standard, è necessario configurare le eccezioni del firewall come richiesto.

## <a name="conclusion"></a>Conclusione

A questo punto, il server web è pronto per accettare e installare i pacchetti web da un computer remoto. Prima di tentare di distribuire un'applicazione web al server, si consiglia di verificare i seguenti punti chiave:

- È stato registrato l'ASP.NET 4.0 con IIS?
- L'identità del pool di applicazioni dispone dell'accesso in lettura alla cartella di origine per il sito Web?
- Il servizio agente di distribuzione Web è in esecuzione?

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni su come configurare i file di progetto di Microsoft Build Engine (MSBuild) personalizzati per distribuire i pacchetti web al servizio agente remoto, vedere [configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Precedente](scenario-configuring-a-production-environment-for-web-deployment.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
