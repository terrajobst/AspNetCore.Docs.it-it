---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Configurazione di un Server Web per il Web la pubblicazione di distribuzione (gestore distribuzione Web) | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come configurare un server web Internet Information Services (IIS) per supportare la distribuzione utilizzando il Han di distribuzione Web di IIS e pubblicazione sul web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: d98be2859181e014ad332298ee3a572ad4235649
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887285"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Configurazione di un Server Web per il Web la pubblicazione di distribuzione (gestore distribuzione Web)
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare un server web Internet Information Services (IIS) per supportare la pubblicazione sul web e la distribuzione utilizzando il gestore di distribuzione Web di IIS.
> 
> Quando si lavora con distribuzione Web 2.0 o versione successiva, sono disponibili tre approcci principali, che è possibile utilizzare per ottenere le applicazioni o siti su un server web. È possibile:
> 
> - Utilizzare il *servizio remoto dell'agente di distribuzione Web*. Questo approccio richiede la configurazione del server web, ma è necessario fornire le credenziali dell'amministratore del server locale per poter distribuire qualsiasi elemento nel server.
> - Utilizzare il *gestore distribuzione Web*. Questo approccio è molto più complesso e richiede uno sforzo iniziale per configurare il server web. Tuttavia, quando si utilizza questo approccio, è possibile configurare IIS per consentire agli utenti senza privilegi di amministratore eseguire la distribuzione. Il gestore di distribuzione Web è disponibile solo in IIS 7 o versione successiva.
> - Utilizzare *distribuzione non in linea*. Questo approccio richiede la minima configurazione del server web, ma un amministratore del server deve copiare il pacchetto web sul server e importarlo tramite Gestione IIS manualmente.
> 
> Per ulteriori informazioni sulla funzionalità chiave, vantaggi e svantaggi di questi approcci, vedere [scelta dell'approccio di destra per distribuzione Web](choosing-the-right-approach-to-web-deployment.md).


Sì, se si desidera consentire agli utenti non amministratori di distribuire il contenuto a specifici siti Web IIS. Questo approccio risulta spesso utile in questi tipi di scenari:

- Gestione temporanea o produzione ambienti in cui l'account utente o un servizio che attiva la distribuzione remota viene in genere non hanno accesso alle credenziali di amministratore del server.
- Ambienti host, in cui si desidera offrire agli utenti remoti la possibilità di aggiornare i siti Web senza concedere loro il controllo completo del server web (o accesso ai siti Web di terzi).

Negli scenari di sviluppo o test o in organizzazioni di piccole dimensioni, la distribuzione di contenuto utilizzando le credenziali di amministratore di server è spesso minore oggetto di controversia. In questi scenari, la configurazione dei server web per supportare la distribuzione utilizzando il [Web distribuire il servizio agente remoto](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) offre un approccio più semplice.

## <a name="task-overview"></a>Panoramica di Task

Per configurare il server web per accettare e distribuire pacchetti web da un computer remoto utilizzando l'approccio gestore distribuzione Web, è necessario:

- Creare o scegliere un account utente (il "utente non amministratore") le cui credenziali da usare per eseguire le distribuzioni.
- Installare IIS 7.5, inclusi il servizio gestione Web e il modulo di autenticazione di base.
- Installare distribuzione Web 2.1 o versione successiva.
- Configurare il servizio di gestione Web per consentire connessioni remote e avviare il servizio.
- Creare un sito Web IIS per ospitare il contenuto distribuito.
- Concedere le autorizzazioni utente non amministratore per il sito Web in Gestione IIS.
- Verificare che il servizio di gestione Web le regole di delega consentono al servizio di aggiungere e modificare il contenuto del sito Web con l'account utente non amministratore.
- Configurare eventuali firewall per consentire le connessioni in ingresso sulla porta 8172.

Per ospitare la soluzione di esempio ContactManager in particolare, è anche necessario:

- Installare .NET Framework 4.0.
- Installare ASP.NET MVC 3.

Questo argomento viene illustrato come eseguire ognuna di queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono che si sta iniziando con una compilazione pulita server che esegue Windows Server 2008 R2. Prima di continuare, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili vengono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e registrazione](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Installare i prodotti e componenti

In questa sezione vi assisterà durante l'installazione di componenti e i prodotti necessari nel server web. Prima di iniziare, una procedura consigliata consiste nell'eseguire Windows Update per verificare che il server è completamente aggiornato.

In questo caso, è necessario installare quanto segue:

- **Configurazione consigliata per IIS 7**. In questo modo il **Server Web (IIS)** ruolo sul server web e installa il set di moduli IIS e i componenti necessari per ospitare un'applicazione ASP.NET.
- **IIS: Servizio di gestione**. Il servizio di gestione Web (WMSvc) vengono installati in IIS. Questo servizio consente la gestione remota dei siti Web IIS ed espone l'endpoint di gestore distribuzione Web ai client.
- **IIS: L'autenticazione di base**. Consente di installare il modulo di autenticazione di base di IIS. Il servizio di gestione Web (WMSvc) Questo consente di autenticare le credenziali specificate.
- **2.1 o successivo dello strumento di distribuzione Web**. Questo modo vengono installati sul server di distribuzione Web (e il relativo file eseguibile sottostante, MSDeploy.exe). Come parte di questo processo, il gestore di distribuzione Web installa e si integra con il servizio di gestione Web.
- **.NET Framework 4.0**. Ciò è necessario per eseguire applicazioni che sono state compilate in questa versione di .NET Framework.
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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. Nel **ASP.NET MVC 3 (Visual Studio 2010)** di riga, fare clic su **Aggiungi**.
7. Nel riquadro di spostamento, fare clic su **Server**.
8. Nel **configurazione consigliata di IIS 7** di riga, fare clic su **Aggiungi**.
9. Nel **2.1 dello strumento di distribuzione Web** di riga, fare clic su **Aggiungi**.
10. Nel **IIS: autenticazione di base** di riga, fare clic su **Aggiungi**.
11. Nel **IIS: servizio di gestione** di riga, fare clic su **Aggiungi**.
12. Fare clic su **Installa**. L'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;con le dipendenze associate&#x2014;sia installato e verrà richiesto di accettare le condizioni di licenza.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Esaminare le condizioni di licenza e, se accettano le condizioni, fare clic su **accetto**.
14. Una volta completato l'installazione, fare clic su **fine**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

Se è installato .NET Framework 4.0 prima di installare IIS, è necessario eseguire il [strumento di registrazione ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) per registrare la versione più recente di ASP.NET con IIS. Se non eseguire questa operazione, sono disponibili che IIS verrà contenuto statico (ad esempio file HTML) senza problemi, ma restituirà **404.0 Errore HTTP: non trovato** quando si tenta di passare al contenuto ASP.NET. È possibile utilizzare la procedura seguente per verificare che sia registrato ASP.NET 4.0.

**Per registrare ASP.NET 4.0 con IIS**

1. Fare clic su **avviare**, quindi digitare **prompt dei comandi**.
2. Nei risultati della ricerca, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.
3. Nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Se si prevede di ospitare applicazioni web a 64 bit in qualsiasi momento, è inoltre necessario registrare la versione a 64 bit di ASP.NET con IIS. A tale scopo, nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

È buona norma, utilizzare Windows Update nuovamente a questo punto per scaricare e installare eventuali aggiornamenti disponibili per i nuovi prodotti e componenti di cui che è installato.

## <a name="configure-the-web-management-service"></a>Configurare il servizio di gestione Web

Ora che è stato installato tutto ciò che occorre, il passaggio successivo consiste nel configurare il servizio gestione Web in IIS. In generale, è necessario completare queste attività:

- Abilitare l'autenticazione di base a livello di server.
- Configurare il servizio di gestione Web per accettare le connessioni remote.
- Avviare il servizio di gestione Web.
- Verificare che le regole di delega di servizio di gestione Web richieste siano soddisfatti.

**Per configurare il servizio di gestione Web**

1. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)**.
2. In Gestione IIS, nel **connessioni** riquadro, fare clic sul nodo server (ad esempio, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Nel riquadro centrale, in **IIS**, fare doppio clic su **autenticazione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. Fare doppio clic su **l'autenticazione di base**, quindi fare clic su **abilitare**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. Nel **connessioni** riquadro, fare clic sul nodo server per tornare alle impostazioni del livello superiore.
6. Nel riquadro centrale, in **Management**, fare doppio clic su **servizio di gestione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Nel riquadro centrale selezionare **abilitare le connessioni remote**.

    > [!NOTE]
    > Se il servizio gestione Web è già in esecuzione, è necessario arrestarlo prima.
8. Nel **azioni** riquadro, fare clic su **avviare** per avviare il servizio di gestione Web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Se viene chiesto di salvare le impostazioni, fare clic su **Sì**.

    > [!NOTE]
    > È consigliabile anche configurare il servizio per l'avvio automatico. A tale scopo, aprire la console servizi, fare doppio clic su **servizio di gestione Web**, quindi fare clic su **proprietà**. Nel **tipo di avvio** elenco a discesa, seleziona **automatica**, quindi fare clic su **OK**.
10. Nel **connessioni** riquadro, fare clic sul nodo server per tornare alle impostazioni del livello superiore.
11. Nel riquadro centrale, in **Management**, fare doppio clic su **Gestione servizio delega**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Verificare che il riquadro centrale contiene un set di regole.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Queste regole consentono agli utenti autorizzati di servizio di gestione Web di utilizzare diversi provider di distribuzione Web. Ad esempio, per distribuire applicazioni web e i contenuti a IIS tramite il gestore di distribuzione Web, deve esistere una regola di delega che consente a tutti gli utenti di servizio di gestione Web per utilizzare autenticati di **contentPath** e **iisApp**  provider (ultima regola che è possibile visualizzare nella schermata).

    Se è installato prodotti e componenti nell'ordine descritto in questo argomento, la versione più recente di distribuzione Web deve aggiunge automaticamente tutte le regole di delega richiesta al servizio di gestione Web. Se la pagina di delega di servizio di gestione non vengono visualizzati tutte le regole, è necessario crearle manualmente. Per istruzioni su come eseguire questa operazione, vedere [configurare il gestore di distribuzione Web](https://go.microsoft.com/?linkid=9805124).
13. Nel **connessioni** riquadro, fare clic sul nodo server per tornare alle impostazioni del livello superiore.

## <a name="create-and-configure-an-iis-website"></a>Creare e configurare un sito Web IIS

Prima di poter distribuire il contenuto web al server, è necessario creare e configurare un sito Web IIS per ospitare il contenuto. Distribuzione Web è possibile distribuire solo pacchetti web a un sito Web IIS esistente; Impossibile creare il sito Web. È inoltre necessario eseguire una minima configurazione aggiuntiva per consentire l'account senza privilegi di amministratore distribuire contenuto in modalità remota. In generale, è necessario completare queste attività:

- Creare una cartella nel file system per ospitare il contenuto.
- Creare un sito Web IIS per fornire il contenuto e associarlo a una cartella locale.
- Concedere autorizzazioni di lettura per l'identità del pool di applicazioni per la cartella locale.
- Concedere le autorizzazioni di IIS necessari per l'account di dominio che consente di distribuire l'applicazione web.

Anche se non c'è niente di arresto dalla distribuzione del contenuto per il sito Web predefinito in IIS, questo approccio non è consigliato per qualsiasi elemento diverso da scenari di test o dimostrazione. Per simulare un ambiente di produzione, è necessario creare un nuovo sito Web IIS con le impostazioni specifiche per i requisiti dell'applicazione.

**Per creare un sito Web IIS**

1. Nel file system locale, creare una cartella per archiviare il contenuto (ad esempio, **C:\DemoSite**).
2. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)**.
3. In Gestione IIS, nel **connessioni** riquadro espandere il nodo del server (ad esempio, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Fare doppio clic su di **siti** nodo e quindi fare clic su **Aggiungi sito Web**.
5. Nel **nome sito** , digitare un nome per il sito Web IIS (ad esempio, **DemoSite**).
6. Nel **percorso fisico** digitare (o passare a) il percorso di cartella locale (ad esempio, **C:\DemoSite**).
7. Nel **porta** , digitare il numero di porta su cui si desidera ospitare il sito Web (ad esempio, **85**).

    > [!NOTE]
    > I numeri di porta standard sono 80 per HTTP e 443 per HTTPS. Tuttavia, se si ospita il sito Web sulla porta 80, è necessario arrestare il sito Web predefinito prima di poter accedere al sito.
8. Lasciare il **nome Host** casella vuota, a meno che non si desidera configurare un record Domain Name System (DNS) per il sito Web e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > In un ambiente di produzione sarà sicuramente si desidera ospitare il sito Web sulla porta 80 e configurare un'intestazione host, con i record DNS corrispondenti. Per ulteriori informazioni sulla configurazione di IIS 7 intestazioni host, vedere [configurare un'intestazione Host per un sito Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Per ulteriori informazioni sul ruolo Server DNS in Windows Server 2008 R2, vedere [Panoramica del Server DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) e [Server DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Nel riquadro **Azioni** sotto **Modifica sito**, fare clic su **Binding**.
10. Nel **binding sito** la finestra di dialogo, fare clic su **Aggiungi**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. Nel **Aggiungi Binding sito** la finestra di dialogo, impostare il **indirizzo IP** e **porta** in modo che corrisponda alla configurazione del sito esistente.
12. Nel **nome Host** , digitare il nome del server web (ad esempio, **STAGEWEB1**), quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > Consente l'associazione del sito prima di accedere al sito localmente utilizzando l'indirizzo IP e porta o `http://localhost:85`. L'associazione del sito secondo consente di accedere al sito da altri computer nel dominio utilizzando il nome del computer (ad esempio, http://stageweb1:85).
13. Nel **binding sito** la finestra di dialogo, fare clic su **Chiudi**.
14. Nel **connessioni** riquadro, fare clic su **pool di applicazioni**.
15. Nel **pool di applicazioni** riquadro destro il nome del pool di applicazioni e quindi fare clic su **le impostazioni di base**. Per impostazione predefinita, il nome del pool di applicazioni corrisponderà al nome del sito Web (ad esempio, **DemoSite**).
16. Nel **versione di .NET Framework** elenco, selezionare **.NET Framework v 4.0.30319**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > La soluzione di esempio richiede .NET Framework 4.0. Questo non è un requisito per distribuzione Web in generale.

Affinché il sito Web fornire contenuto, l'identità del pool di applicazioni deve disporre delle autorizzazioni per la cartella locale che contiene il contenuto lettura. In IIS 7.5, pool di applicazioni eseguiti con un'identità di pool di applicazione univoco per impostazione predefinita (a differenza delle versioni precedenti di IIS, in cui i pool di applicazioni verrebbero in genere eseguito l'account del servizio di rete). L'identità del pool non è un account utente reale e non vengono visualizzati in qualsiasi elenco di utenti o gruppi&#x2014;viene invece creata in modo dinamico quando viene avviato il pool di applicazioni. Ogni identità di pool di applicazioni viene aggiunto all'oggetto locale **IIS\_IUSRS** gruppo di sicurezza come elemento nascosto.

Per concedere autorizzazioni a un'identità del pool di applicazioni in un file o una cartella, sono disponibili due opzioni:

- Assegnare autorizzazioni all'identità del pool di applicazioni direttamente, utilizzando il formato <strong>IIS AppPool\</ strong ><em>[nome pool di applicazioni]</em>(ad esempio, <strong>IIS AppPool\DemoSite</strong>).
- Assegnare autorizzazioni per il **IIS\_IUSRS** gruppo.

L'approccio più comune consiste nell'assegnare autorizzazioni a locale **IIS\_IUSRS** gruppo, perché questo approccio consente di modificare i pool di applicazioni senza dover riconfigurare le autorizzazioni del file. La procedura successiva Usa questo approccio in base al gruppo.

> [!NOTE]
> Per ulteriori informazioni sull'identità del pool di applicazioni in IIS 7.5, vedere [le identità del Pool di applicazioni](https://go.microsoft.com/?linkid=9805123).


**Per configurare le autorizzazioni per un sito Web IIS**

1. In Esplora risorse, passare al percorso della cartella locale.
2. Fare clic sulla cartella e quindi fare clic su **proprietà**.
3. Nel **sicurezza** scheda, fare clic su **modifica**e quindi fare clic su **Aggiungi**.
4. Fare clic su **percorsi**. Nel **percorsi** la finestra di dialogo, selezionare il server locale, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. Nel **Seleziona utenti o gruppi** della finestra di dialogo tipo **IIS\_IUSRS**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.
6. Nel <strong>le autorizzazioni per</strong><em>[nome cartella]</em> la finestra di dialogo, si noti che il nuovo gruppo è stato assegnato il <strong>lettura &amp; eseguire</strong>, <strong>visualizzazione contenuto cartella contenuto</strong>, e <strong>lettura</strong> le autorizzazioni per impostazione predefinita. Lasciare invariata e fare clic su <strong>OK</strong>.
7. Fare clic su <strong>OK</strong> per chiudere la <em>[nome cartella]</em><strong>proprietà</strong> la finestra di dialogo.

Come attività finale, è necessario concedere le autorizzazioni appropriate per l'utente non amministratore, le cui credenziali da usare per distribuire il contenuto. L'utente richiede le autorizzazioni per distribuire contenuto in remoto al sito Web.

**Per configurare le autorizzazioni del sito Web IIS per un utente non amministratore dominio**

1. In Gestione IIS, nel **connessioni** riquadro, fare doppio clic su un nodo di sito Web (ad esempio, **DemoSite**), scegliere **Distribuisci**e quindi fare clic su **configurare Web La pubblicazione di distribuzione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. Nel **configurare distribuire pubblicazione sul Web** a destra della finestra di dialogo di **selezionare un utente per concedere le autorizzazioni di pubblicazione** elenco, fare clic sul pulsante con i puntini di sospensione.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. Nel **consentire utente** finestra di dialogo digitare il dominio e il nome utente dell'account che si desidera utilizzare per distribuire il contenuto e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. Nel **configurare distribuire pubblicazione sul Web** la finestra di dialogo, fare clic su **installazione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Questa operazione esegue due funzioni chiave in un unico passaggio. In primo luogo, vengono concesse all'utente l'autorizzazione per modificare il sito Web in modalità remota tramite il servizio di gestione Web, secondo le regole di delega che sono stati esaminati nella sezione precedente. In secondo luogo, concede il controllo completo all'utente della cartella di origine per il sito Web, che consente all'utente di aggiungere, modificare e impostare le autorizzazioni per il contenuto del sito Web.
5. Nel **configurare distribuire pubblicazione sul Web** la finestra di dialogo, fare clic su **Chiudi**.

## <a name="configure-firewall-exceptions"></a>Configurare le eccezioni del Firewall

Per impostazione predefinita, il servizio di gestione Web di IIS è in ascolto sulla porta TCP 8172. Se Windows Firewall è abilitato sul server web, è necessario creare una nuova regola di in entrata per consentire il traffico TCP sulla porta 8172 (tutto il traffico in uscita è consentito per impostazione predefinita in Windows Firewall). Se si utilizza un firewall di terze parti, è necessario creare regole per consentire il traffico.

| Direzione | Dalla porta | Alla porta | Tipo di porta |
| --- | --- | --- | --- |
| In ingresso | Qualsiasi | 8172 | TCP |
| In uscita | 8172 | Qualsiasi | TCP |
  

Per ulteriori informazioni sulla configurazione di regole in Windows Firewall, vedere [configurazione delle regole Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Per i firewall di terze parti, consultare la documentazione del prodotto.

## <a name="conclusion"></a>Conclusione

Il server web dovrebbe ora essere pronto per accettare le distribuzioni remote per il gestore distribuzione Web tramite il servizio di gestione Web. Prima di tentare di distribuire un'applicazione web al server, si consiglia di verificare i seguenti punti chiave:

- È stato abilitato l'autenticazione di base a livello di server in IIS?
- Sono state attivate le connessioni remote al servizio di gestione Web?
- Il servizio di gestione Web è stato avviato?
- Sono presenti management le regole di delega di servizio sul posto?
- L'identità del pool di applicazioni dispone dell'accesso in lettura alla cartella di origine per il sito Web?
- L'account utente non amministratore dispone delle autorizzazioni a livello di sito in IIS?
- Il firewall consenta le connessioni in ingresso per il server sulla porta TCP 8172?

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni su come configurare i file di progetto di Microsoft Build Engine (MSBuild) personalizzati per distribuire i pacchetti web per il gestore di distribuzione Web, vedere [configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Precedente](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
