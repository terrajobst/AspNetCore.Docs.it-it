---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Configurazione di un Server Web per il Web distribuire pubblicazione (distribuzione Offline) | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come configurare un server web IIS per supportare la distribuzione e pubblicazione sul web non in linea. Quando si utilizza Internet Information Services (si....
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: cd3343f58cbb9bb868d15a91152f07444c2bd68e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Configurazione di un Server Web per la pubblicazione (distribuzione Offline) di distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare un server web IIS per supportare la distribuzione e pubblicazione sul web non in linea.
> 
> Quando si utilizza Internet Information Services (IIS) strumento di distribuzione Web (distribuzione Web) 2.0 o versione successiva, sono disponibili tre approcci principali, che è possibile utilizzare per ottenere le applicazioni o siti su un server web. È possibile:
> 
> - Utilizzare il *servizio remoto dell'agente di distribuzione Web*. Questo approccio richiede la configurazione del server web, ma è necessario fornire le credenziali dell'amministratore del server locale per poter distribuire qualsiasi elemento nel server.
> - Utilizzare il *gestore distribuzione Web*. Questo approccio è molto più complesso e richiede uno sforzo iniziale per configurare il server web. Tuttavia, quando si utilizza questo approccio, è possibile configurare IIS per consentire agli utenti senza privilegi di amministratore eseguire la distribuzione. Il gestore di distribuzione Web è disponibile solo in IIS 7 o versione successiva.
> - Utilizzare *distribuzione non in linea*. Questo approccio richiede la minima configurazione del server web, ma un amministratore del server deve copiare il pacchetto web sul server e importarlo tramite Gestione IIS manualmente.
> 
> Per ulteriori informazioni sulla funzionalità chiave, vantaggi e svantaggi di questi approcci, vedere [scelta dell'approccio di destra per distribuzione Web](choosing-the-right-approach-to-web-deployment.md).


Sì, se le restrizioni di sicurezza o infrastruttura di rete impediscono la distribuzione remota. Questo è più probabile che sia nel caso di ambienti di produzione con connessione Internet, in cui i server web sono isolati & #x 2014; sia fisicamente o da firewall e le subnet & #x 2014; dal resto dell'infrastruttura server.

Ovviamente, questo approccio diventa meno convenienti se le applicazioni web vengono aggiornate a intervalli regolari. Se è consentito l'infrastruttura, è consigliabile provare ad abilitare la distribuzione remota, utilizzando il gestore di distribuzione Web o distribuire servizio agente remoto di Web.

## <a name="task-overview"></a>Panoramica di Task

Per configurare il server web per il supporto di importazione non in linea e la distribuzione dei pacchetti web, è necessario:

- Installare IIS 7.5 e la configurazione consigliata di IIS 7.
- Installare distribuzione Web 2.1 o versione successiva.
- Creare un sito Web IIS per ospitare il contenuto distribuito.
- Disabilitare il servizio agente di distribuzione Web.

Per ospitare la soluzione di esempio, in particolare, è anche necessario:

- Installare .NET Framework 4.0.
- Installare ASP.NET MVC 3.

Questo argomento viene illustrato come eseguire ognuna di queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono che si sta iniziando con una compilazione pulita server che esegue Windows Server 2008 R2. Prima di continuare, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili vengono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e registrazione](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Installare i prodotti e componenti

In questa sezione vi assisterà durante l'installazione di componenti e i prodotti necessari nel server web. Prima di iniziare, una procedura consigliata consiste nell'eseguire Windows Update per verificare che il server è completamente aggiornato.

In questo caso, è necessario installare quanto segue:

- **Configurazione consigliata per IIS 7**. In questo modo il **Server Web (IIS)** ruolo sul server web e installa il set di moduli IIS e i componenti necessari per ospitare un'applicazione ASP.NET.
- **.NET framework 4.0**. Ciò è necessario per eseguire applicazioni che sono state compilate in questa versione di .NET Framework.
- **2.1 o successivo dello strumento di distribuzione Web**. Questo modo vengono installati sul server di distribuzione Web (e il relativo file eseguibile sottostante, MSDeploy.exe). Distribuzione Web integrato con IIS e consente di importare ed esportare pacchetti web.
- **ASP.NET MVC 3**. Consente di installare gli assembly che necessari per eseguire applicazioni MVC 3.

> [!NOTE]
> Questa procedura dettagliata viene descritto l'utilizzo dell'installazione guidata piattaforma Web per installare e configurare i vari componenti. Anche se non è necessario utilizzare l'installazione guidata piattaforma Web, semplifica il processo di installazione automaticamente il rilevamento delle dipendenze e verificando che si ottengano sempre le versioni del prodotto più recenti. Per ulteriori informazioni, vedere [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/?linkid=9805118).


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

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. Nel **ASP.NET MVC 3 (Visual Studio 2010)** di riga, fare clic su **Aggiungi**.
7. Nel riquadro di spostamento, fare clic su **Server**.
8. Nel **configurazione consigliata di IIS 7** di riga, fare clic su **Aggiungi**.
9. Nel **2.1 dello strumento di distribuzione Web** di riga, fare clic su **Aggiungi**.
10. Fare clic su **Installa**. L'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti & #x 2014; insieme a tutte le dipendenze associate & #x 2014; sia installato e verrà richiesto di accettare le condizioni di licenza.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Esaminare le condizioni di licenza e, se accettano le condizioni, fare clic su **accetto**.
12. Una volta completato l'installazione, fare clic su **fine**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

Se è installato .NET Framework 4.0 prima di installare IIS, è necessario eseguire il [strumento di registrazione ASP.NET IIS](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) per registrare la versione più recente di ASP.NET con IIS. Se non eseguire questa operazione, sono disponibili che IIS verrà contenuto statico (ad esempio file HTML) senza problemi, ma restituirà **404.0 Errore HTTP: non trovato** quando si tenta di passare al contenuto ASP.NET. È possibile utilizzare la procedura seguente per verificare che sia registrato ASP.NET 4.0.

**Per registrare ASP.NET 4.0 con IIS**

1. Fare clic su **avviare**, quindi digitare **prompt dei comandi**.
2. Nei risultati della ricerca, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.
3. Nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Se si prevede di ospitare applicazioni web a 64 bit in qualsiasi momento, è inoltre necessario registrare la versione a 64 bit di ASP.NET con IIS. A tale scopo, nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

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
3. In Gestione IIS, nel **connessioni** riquadro espandere il nodo del server (ad esempio, **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Fare doppio clic su di **siti** nodo e quindi fare clic su **Aggiungi sito Web**.
5. Nel **nome sito** , digitare un nome per il sito Web IIS (ad esempio, **DemoSite**).
6. Nel **percorso fisico** digitare (o passare a) il percorso di cartella locale (ad esempio, **C:\DemoSite**).
7. Nel **porta** , digitare il numero di porta su cui si desidera ospitare il sito Web (ad esempio, **85**).

    > [!NOTE]
    > I numeri di porta standard sono 80 per HTTP e 443 per HTTPS. Tuttavia, se si ospita il sito Web sulla porta 80, è necessario arrestare il sito Web predefinito prima di poter accedere al sito.
8. Lasciare il **nome Host** casella vuota, a meno che non si desidera configurare un record Domain Name System (DNS) per il sito Web e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > In un ambiente di produzione sarà sicuramente si desidera ospitare il sito Web sulla porta 80 e configurare un'intestazione host, con i record DNS corrispondenti. Per ulteriori informazioni sulla configurazione di IIS 7 intestazioni host, vedere [configurare un'intestazione Host per un sito Web (IIS 7)](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx). Per ulteriori informazioni sul ruolo Server DNS in Windows Server 2008 R2, vedere [Panoramica del Server DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) e [Server DNS](https://technet.microsoft.com/en-us/windowsserver/dd448607).
9. Nel riquadro **Azioni** sotto **Modifica sito**, fare clic su **Binding**.
10. Nel **binding sito** la finestra di dialogo, fare clic su **Aggiungi**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. Nel **Aggiungi Binding sito** la finestra di dialogo, impostare il **indirizzo IP** e **porta** in modo che corrisponda alla configurazione del sito esistente.
12. Nel **nome Host** , digitare il nome del server web (ad esempio, **PROWEB1**), quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > Consente l'associazione del sito prima di accedere al sito localmente utilizzando l'indirizzo IP e porta o `http://localhost:85`. La seconda associazione del sito consente di accedere al sito da altri computer nel dominio utilizzando il nome del computer (ad esempio, http://proweb1:85).
13. Nel **binding sito** la finestra di dialogo, fare clic su **Chiudi**.
14. Nel **connessioni** riquadro, fare clic su **pool di applicazioni**.
15. Nel **pool di applicazioni** riquadro destro il nome del pool di applicazioni e quindi fare clic su **le impostazioni di base**. Per impostazione predefinita, il nome del pool di applicazioni corrisponderà al nome del sito Web (ad esempio, **DemoSite**).
16. Nel **versione di .NET Framework** elenco, selezionare **.NET Framework v 4.0.30319**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > La soluzione di esempio richiede .NET Framework 4.0. Questo non è un requisito per distribuzione Web in generale.

Affinché il sito Web fornire contenuto, l'identità del pool di applicazioni deve disporre delle autorizzazioni per la cartella locale che contiene il contenuto lettura. In IIS 7.5, pool di applicazioni eseguiti con un'identità di pool di applicazione univoco per impostazione predefinita (a differenza delle versioni precedenti di IIS, in cui i pool di applicazioni verrebbero in genere eseguito l'account del servizio di rete). L'identità del pool di applicazioni non è un account utente reale e non vengono visualizzati in qualsiasi elenco di utenti o i gruppi & #x 2014; al contrario, viene creato in modo dinamico quando viene avviato il pool di applicazioni. Ogni identità di pool di applicazioni viene aggiunto all'oggetto locale **IIS\_IUSRS** gruppo di sicurezza come elemento nascosto.

Per concedere autorizzazioni a un'identità del pool di applicazioni in un file o una cartella, sono disponibili due opzioni:

- Assegnare autorizzazioni all'identità del pool di applicazioni direttamente, utilizzando il formato **IIS AppPool\***[nome pool di applicazioni] * (ad esempio, **IIS AppPool\DemoSite**).
- Assegnare autorizzazioni per il **IIS\_IUSRS** gruppo.

L'approccio più comune consiste nell'assegnare autorizzazioni a locale **IIS\_IUSRS** gruppo, perché questo approccio consente di modificare i pool di applicazioni senza dover riconfigurare le autorizzazioni del file. La procedura successiva Usa questo approccio in base al gruppo.

> [!NOTE]
> Per ulteriori informazioni sull'identità del pool di applicazioni in IIS 7.5, vedere [le identità del Pool di applicazioni](https://go.microsoft.com/?linkid=9805123).


**Per configurare le autorizzazioni per un sito Web IIS**

1. In Esplora risorse, passare al percorso della cartella locale.
2. Fare clic sulla cartella e quindi fare clic su **proprietà**.
3. Nel **sicurezza** scheda, fare clic su **modifica**e quindi fare clic su **Aggiungi**.
4. Fare clic su **percorsi**. Nel **percorsi** la finestra di dialogo, selezionare il server locale, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. Nel **Seleziona utenti o gruppi** della finestra di dialogo tipo **IIS\_IUSRS**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.
6. Nel **le autorizzazioni per***[nome cartella]* la finestra di dialogo, si noti che il nuovo gruppo è stato assegnato il **lettura &amp; eseguire**, **visualizzazione contenuto cartella contenuto**, e **lettura** le autorizzazioni per impostazione predefinita. Lasciare invariata e fare clic su **OK**.
7. Fare clic su **OK** per chiudere la *[nome cartella]***proprietà** la finestra di dialogo.

## <a name="disable-the-remote-agent-service"></a>Disabilitare il servizio agente remoto

Quando si installa distribuzione Web, il servizio agente di distribuzione Web è installato e avviato automaticamente. Questo servizio consente di distribuire e pubblicare i pacchetti web da una posizione remota. Non è possibile utilizzare la funzionalità di distribuzione remoto in questo scenario, in modo da arrestare e disabilitare il servizio.

> [!NOTE]
> Non è necessario arrestare il servizio agente remoto per importare e distribuire manualmente un pacchetto di web. Tuttavia, è consigliabile arrestare e disabilitare il servizio se non si intende utilizzarlo.


È possibile arrestare e disabilitare un servizio in più modi, mediante diverse utilità della riga di comando o i cmdlet di Windows PowerShell. Questa procedura viene descritto un approccio basato su interfaccia utente semplice.

**Per arrestare e disabilitare il servizio agente remoto**

1. Scegliere **Start** , **Strumenti di amministrazione**, quindi scegliere **Servizi**.
2. Nella console servizi, individuare il **servizio agente distribuzione Web** riga.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Fare doppio clic su **servizio agente distribuzione Web**, quindi fare clic su **proprietà**.
4. Nel **proprietà del servizio agente distribuzione Web** la finestra di dialogo, fare clic su **arrestare**.
5. Nel **tipo di avvio** elenco, selezionare **disabilitato**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Conclusione

A questo punto, il server web è pronto per la distribuzione di pacchetto web offline. Prima di tentare di importare i pacchetti web in un sito Web IIS, si consiglia di verificare i seguenti punti chiave:

- È stato registrato l'ASP.NET 4.0 con IIS?
- L'identità del pool di applicazioni dispone dell'accesso in lettura alla cartella di origine per il sito Web?
- Non il servizio agente di distribuzione Web?

>[!div class="step-by-step"]
[Precedente](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[Successivo](configuring-a-database-server-for-web-deploy-publishing.md)
