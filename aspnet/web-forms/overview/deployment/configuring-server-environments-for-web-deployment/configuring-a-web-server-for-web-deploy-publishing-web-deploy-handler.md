---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Configurazione di un Server Web per il Web pubblicazione con distribuzione (gestore di distribuzione Web) | Microsoft Docs
author: jrjlee
description: Questo argomento descrive come configurare un server web Internet Information Services (IIS) per supportare la pubblicazione sul web e la distribuzione usando il Han di distribuzione Web di IIS...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 3296bb9b6460bbe80782746c9d398aa67815dcee
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833335"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Configurazione di un Server Web per il Web pubblicazione con distribuzione (gestore di distribuzione Web)
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come configurare un server web Internet Information Services (IIS) per supportare la pubblicazione sul web e la distribuzione usando il gestore di distribuzione Web di IIS.
> 
> Quando si lavora con distribuzione Web 2.0 o versione successiva, esistono tre approcci principali, che è possibile usare per ottenere le applicazioni o i siti in un server web. È possibile:
> 
> - Usare la *assistenza remota dell'agente di distribuzione Web*. Questo approccio implica una configurazione del server web, ma è necessario fornire le credenziali dell'amministratore del server locale per distribuire qualsiasi elemento nel server.
> - Usare la *gestore distribuzione Web*. Questo approccio è molto più complesso e richiede più di impegno iniziale a configurare il server web. Tuttavia, quando si usa questo approccio, è possibile configurare IIS per consentire agli utenti senza privilegi di amministratore eseguire la distribuzione. Il gestore di distribuzione Web è disponibile solo in IIS 7 o versione successiva.
> - Uso *distribuzione offline*. Questo approccio richiede la minima configurazione del server web, ma un amministratore del server deve copiare il pacchetto web nel server e importarlo tramite Gestione IIS manualmente.
> 
> Per altre informazioni su funzionalità chiave, vantaggi e gli svantaggi di questi approcci, vedere [scelta dell'approccio a destra per la distribuzione Web](choosing-the-right-approach-to-web-deployment.md).


Sì, se si desidera consentire agli utenti senza privilegi di amministratore distribuire il contenuto a specifici siti Web IIS. Questo approccio è spesso utile in questi tipi di scenari:

- Gestione temporanea o produzione ambienti, in cui l'account utente o un servizio che attiva la distribuzione remota viene in genere non hanno accesso alle credenziali di amministratore del server.
- Ambienti host, in cui si desidera consentire a utenti remoti per aggiornare i siti Web senza concedere loro il controllo completo del server web (o l'accesso a siti Web di altri utenti).

Negli scenari di sviluppo o test, o in organizzazioni di piccole dimensioni, distribuzione del contenuto usando le credenziali di amministratore di server è spesso inferiore controverso. In questi scenari, la configurazione dei server web per supportare la distribuzione usando il [Web distribuire il servizio agente remoto](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) offre un approccio più semplice.

## <a name="task-overview"></a>Panoramica di Task

Per configurare il server web per accettare e distribuire i pacchetti web da un computer remoto usando l'approccio gestore distribuzione Web, è necessario:

- Creare o scegliere, account utente di dominio (il "utente senza privilegi di amministratore") le cui credenziali si userà per eseguire le distribuzioni.
- Installare IIS 7.5, incluso il servizio di gestione Web e il modulo di autenticazione di base.
- Installare la funzionalità distribuzione Web 2.1 o versioni successive.
- Configurare il servizio di gestione Web per consentire le connessioni remote e avviare il servizio.
- Creare un sito Web IIS per ospitare il contenuto distribuito.
- Concedere le autorizzazioni degli utenti senza privilegi di amministratore nel sito Web in Gestione IIS.
- Verificare che il servizio di gestione Web le regole di delega consentono al servizio di aggiungere e modificare contenuto del sito Web usando l'account utente non amministratore.
- Configurare eventuali firewall per consentire le connessioni in ingresso sulla porta 8172.

Per ospitare la soluzione di esempio ContactManager in particolare, è anche necessario:

- Installare .NET Framework 4.0.
- Installare ASP.NET MVC 3.

In questo argomento illustrerà come eseguire ognuna di queste procedure. Le attività e procedure dettagliate in questo argomento presuppongono che si intende iniziare con una compilazione pulita server che esegue Windows Server 2008 R2. Prima di continuare, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.
- Il server viene aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio ed effettuare l'accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per altre informazioni su come configurare gli indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Installare i prodotti e componenti

In questa sezione consentirà di installare i componenti e prodotti necessari nel server web. Prima di iniziare, è consigliabile eseguire Windows Update per verificare che il server sia completamente aggiornato.

In questo caso, è necessario installare quanto segue:

- **Configurazione consigliata per IIS 7**. In questo modo, il **Server Web (IIS)** ruolo sul server web e installa il set di moduli IIS e i componenti necessari per ospitare un'applicazione ASP.NET.
- **IIS: Servizio di gestione**. Ciò consente di installare il servizio di gestione Web (WMSvc) in IIS. Questo servizio consente la gestione remota di siti Web IIS ed espone l'endpoint di gestore distribuzione Web ai client.
- **IIS: L'autenticazione di base**. Ciò consente di installare il modulo di autenticazione di base di IIS. Questo servizio di gestione Web (WMSvc) consente di eseguire l'autenticazione le credenziali specificate.
- **Web Deployment Tool 2.1 o versioni successive**. Ciò consente di installare distribuzione Web (e il relativo file eseguibile sottostante, MSDeploy.exe) nel server. Come parte di questo processo, installa il gestore di distribuzione Web e si integra con il servizio di gestione Web.
- **.NET Framework 4.0**. Ciò è necessario eseguire le applicazioni che sono state create in questa versione di .NET Framework.
- **ASP.NET MVC 3**. Ciò consente di installare gli assembly che è necessario eseguire le applicazioni MVC 3.

> [!NOTE]
> Questa procedura dettagliata viene descritto l'utilizzo dell'installazione guidata piattaforma Web per installare e configurare i vari componenti. Sebbene non sia necessario usare l'installazione guidata piattaforma Web, semplifica il processo di installazione automaticamente rilevando le dipendenze e assicurare che ottengano sempre le versioni di prodotto più recenti. Per altre informazioni, vedere [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/?linkid=9805118).


**Per installare i componenti e prodotti necessari**

1. Scaricare e installare il [instalace Webové Platformy](https://go.microsoft.com/?linkid=9805118).
2. Durante l'installazione è stata completata, verrà avviata automaticamente dall'installazione guidata piattaforma Web.

    > [!NOTE]
    > È ora possibile avviare l'installazione guidata piattaforma Web in qualsiasi momento dal **avviare** menu. A tale scopo, scegliere il **avviare** menu, fare clic su **tutti i programmi**e quindi fare clic su **Microsoft Web Platform Installer**.
3. In cima il **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
4. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Frameworks**.
5. Nel **Microsoft .NET Framework 4** righe, se .NET Framework non è già installato, fare clic su **Add**.

    > [!NOTE]
    > Si potrebbe avere già installato .NET Framework 4.0 tramite Windows Update. Se è già installato un prodotto o componente, l'installazione guidata piattaforma Web indicherà questo sostituendo il **Add** sul pulsante con il testo **installato**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. Nel **ASP.NET MVC 3 (Visual Studio 2010)** riga, fare clic su **Add**.
7. Nel riquadro di spostamento, fare clic su **Server**.
8. Nel **la configurazione consigliata di IIS 7** riga, fare clic su **Add**.
9. Nel **2.1 dello strumento di distribuzione Web** riga, fare clic su **Add**.
10. Nel **IIS: autenticazione di base** riga, fare clic su **Add**.
11. Nel **IIS: servizio di gestione** riga, fare clic su **Add**.
12. Fare clic su **Installa**. L'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;con le dipendenze associate&#x2014;per essere installata e verrà richiesto di accettare le condizioni di licenza.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Esaminare le condizioni di licenza e se l'utente accetti le condizioni, fare clic su **accetto**.
14. Una volta completata l'installazione, fare clic su **Finish**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

Se è installato .NET Framework 4.0 prima dell'installazione di IIS, è necessario eseguire la [strumento di registrazione ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) per registrare la versione più recente di ASP.NET con IIS. Se si non esegue questa operazione, si scoprirà che IIS fornirà il contenuto statico (ad esempio file HTML) senza problemi, ma restituirà **404.0 Errore HTTP – non trovato** quando si tenta di passare al contenuto ASP.NET. È possibile utilizzare la procedura seguente per assicurarsi che sia registrato ASP.NET 4.0.

**Per registrare ASP.NET 4.0 con IIS**

1. Fare clic su **avviare**, quindi digitare **prompt dei comandi**.
2. Nei risultati della ricerca, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.
3. Nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Se si prevede di ospitare applicazioni web a 64 bit in qualsiasi momento, è consigliabile registrare anche la versione a 64 bit di ASP.NET con IIS. A tale scopo, nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Come buona pratica, utilizzare Windows Update nuovamente a questo punto per scaricare e installare eventuali aggiornamenti disponibili per i nuovi prodotti e componenti installati.

## <a name="configure-the-web-management-service"></a>Configurare il servizio di gestione Web

Ora che è stato installato tutto ciò che occorre, il passaggio successivo consiste nel configurare il servizio di gestione Web in IIS. A livello generale, è necessario completare queste attività:

- Abilitare l'autenticazione di base a livello di server.
- Configurare il servizio di gestione Web per accettare le connessioni remote.
- Avviare il servizio di gestione Web.
- Verificare che le regole di delega di servizio di gestione Web necessari siano soddisfatti.

**Per configurare il servizio di gestione Web**

1. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)**.
2. In Gestione IIS nel **connessioni** riquadro, fare clic sul nodo del server (ad esempio, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Nel riquadro centrale, sotto **IIS**, fare doppio clic su **autenticazione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. Fare doppio clic su **l'autenticazione di base**, quindi fare clic su **abilitare**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. Nel **connessioni** riquadro, fare clic sul nodo del server per tornare alle impostazioni di primo livello.
6. Nel riquadro centrale, sotto **Management**, fare doppio clic su **servizio di gestione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Nel riquadro centrale, selezionare **abilitare le connessioni remote**.

    > [!NOTE]
    > Se il servizio di gestione Web è già in esecuzione, è necessario arrestarla prima di tutto.
8. Nel **azioni** riquadro, fare clic su **avviare** per avviare il servizio di gestione Web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Se viene chiesto di salvare le impostazioni, fare clic su **Sì**.

    > [!NOTE]
    > È anche possibile configurare il servizio venga avviato automaticamente. A tale scopo, aprire la console servizi, fare doppio clic su **servizio di gestione Web**, quindi fare clic su **proprietà**. Nel **tipo di avvio** elenco a discesa, seleziona **automatici**, quindi fare clic su **OK**.
10. Nel **connessioni** riquadro, fare clic sul nodo del server per tornare alle impostazioni di primo livello.
11. Nel riquadro centrale, sotto **Management**, fare doppio clic su **delega del servizio Gestione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Verificare che il riquadro centrale contiene un set di regole.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Queste regole consentono agli utenti autorizzati di servizio di gestione Web di usare diversi provider di distribuzione Web. Ad esempio, per distribuire le applicazioni web e il contenuto a IIS tramite il gestore di distribuzione Web, deve esistere una regola di delega che consente a tutti gli utenti di servizio di gestione Web usare autenticati il **contentPath** e **iisApp**  provider (l'ultima regola che è possibile visualizzare nella schermata).

    Se è installato prodotti e componenti nell'ordine descritto in questo argomento, la versione più recente di distribuzione Web deve aggiungere automaticamente tutte le regole di delega necessaria al servizio di gestione Web. Se la pagina di gestione del servizio delega non viene visualizzata alcuna regola, è necessario crearle manualmente. Per istruzioni su come eseguire questa operazione, vedere [configurare il gestore di distribuzione Web](https://go.microsoft.com/?linkid=9805124).
13. Nel **connessioni** riquadro, fare clic sul nodo del server per tornare alle impostazioni di primo livello.

## <a name="create-and-configure-an-iis-website"></a>Creare e configurare un sito Web IIS

Prima di poter distribuire il contenuto web al server, è necessario creare e configurare un sito Web IIS per ospitare il contenuto. La funzionalità distribuzione Web è possibile distribuire solo pacchetti web a un sito Web IIS esistente; Impossibile creare il sito Web per l'utente. È anche necessario eseguire una configurazione aggiuntiva little per consentire l'account senza privilegi di amministratore distribuire il contenuto in modalità remota. A livello generale, è necessario completare queste attività:

- Creare una cartella nel file system per ospitare il contenuto.
- Creare un sito Web IIS per rendere disponibile il contenuto e associarlo a una cartella locale.
- Concessione di autorizzazioni di lettura per l'identità del pool di applicazioni per la cartella locale.
- Concedere le autorizzazioni di IIS necessari per l'account di dominio che verrà distribuita l'applicazione web.

Anche se nulla che vieti di distribuzione del contenuto al sito Web predefinito in IIS, questo approccio non è consigliato per qualsiasi elemento diverso da scenari di test o dimostrazione. Per simulare un ambiente di produzione, è necessario creare un nuovo sito Web IIS con impostazioni specifiche per i requisiti dell'applicazione.

**Per creare un sito Web IIS**

1. Nel file system locale, creare una cartella per archiviare il contenuto (ad esempio, **C:\DemoSite**).
2. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)**.
3. In Gestione IIS nel **connessioni** riquadro, espandere il nodo del server (ad esempio, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Fare doppio clic il **siti** nodo e quindi fare clic su **Aggiungi sito Web**.
5. Nel **nomesito** , digitare un nome per il sito Web IIS (ad esempio, **DemoSite**).
6. Nel **percorso fisico** digitare (o passare a) il percorso alla cartella locale (ad esempio **C:\DemoSite**).
7. Nel **porta** , digitare il numero di porta su cui si vuole ospitare il sito Web (ad esempio **85**).

    > [!NOTE]
    > I numeri di porta standard sono 80 per HTTP e 443 per HTTPS. Tuttavia, se si ospita il sito Web sulla porta 80, è necessario arrestare il sito Web predefinito prima che si possano accedere al sito.
8. Lasciare il **nome Host** casella vuota, a meno che non si desidera configurare un record di sistema DNS (Domain Name) per il sito Web e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > In un ambiente di produzione probabilmente vorrai ospitare il sito Web sulla porta 80 e configurare un'intestazione host, insieme ai record DNS corrispondenti. Per altre informazioni su come configurare le intestazioni host in IIS 7, vedere [configurare un'intestazione Host per un sito Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Per altre informazioni sul ruolo Server DNS in Windows Server 2008 R2, vedere [Panoramica di Server DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) e [Server DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Nel riquadro **Azioni** sotto **Modifica sito**, fare clic su **Binding**.
10. Nel **binding sito** finestra di dialogo, fare clic su **Aggiungi**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. Nel **Aggiungi Binding sito** finestra di dialogo, impostare il **indirizzo IP** e **porta** in modo che corrisponda alla configurazione del sito esistente.
12. Nel **nome Host** , digitare il nome del server web (ad esempio, **STAGEWEB1**), quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > Il primo binding del sito consente di accedere al sito in locale usando l'indirizzo IP e la porta o `http://localhost:85`. L'associazione del sito secondo consente di accedere al sito da altri computer appartenenti al dominio usando il nome del computer (ad esempio, http://stageweb1:85).
13. Nel **binding sito** finestra di dialogo, fare clic su **Chiudi**.
14. Nel **connessioni** riquadro, fare clic su **pool di applicazioni**.
15. Nel **pool di applicazioni** riquadro, fare doppio clic il nome del pool di applicazioni e quindi fare clic su **impostazioni di base**. Per impostazione predefinita, il nome del pool di applicazioni corrisponderà al nome del sito Web (ad esempio, **DemoSite**).
16. Nel **versione di .NET Framework** elenco, selezionare **.NET Framework v4.0.30319**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > La soluzione di esempio richiede .NET Framework 4.0. Questo non è un requisito per la distribuzione Web in generale.

In ordine per il sito Web rendere disponibile il contenuto, l'identità del pool di applicazioni deve avere autorizzazioni lettura per la cartella locale che archivia il contenuto. In IIS 7.5, eseguire i pool di applicazioni con un'identità di pool di applicazioni univoche per impostazione predefinita (a differenza delle versioni precedenti di IIS, in cui i pool di applicazioni vengono in genere eseguito usando l'account del servizio di rete). L'identità del pool non è un account utente reale e non viene visualizzato in qualsiasi elenco di utenti o gruppi&#x2014;viene invece creata in modo dinamico quando viene avviato il pool di applicazioni. Ogni identità del pool di applicazioni viene aggiunto a locale **IIS\_IUSRS** gruppi di sicurezza come un elemento nascosto.

Per concedere autorizzazioni a un'identità del pool di applicazioni in un file o una cartella, sono disponibili due opzioni:

- Assegnare le autorizzazioni all'identità del pool di applicazioni direttamente, usando il formato <strong>IIS AppPool\</ strong ><em>[nome pool di applicazioni]</em>(ad esempio, <strong>IIS AppPool\DemoSite</strong>).
- Assegnare autorizzazioni per il **IIS\_IUSRS** gruppo.

L'approccio più comune consiste nell'assegnare autorizzazioni a locale **IIS\_IUSRS** raggruppare, perché questo approccio consente di modificare i pool di applicazioni senza dover riconfigurare le autorizzazioni del file. La procedura successiva Usa questo approccio basato su gruppo.

> [!NOTE]
> Per ulteriori informazioni sull'identità del pool di applicazioni in IIS 7.5, vedere [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).


**Configurare le autorizzazioni di cartella per un sito Web IIS**

1. In Windows Explorer passare al percorso della cartella locale.
2. Pulsante destro del mouse nella cartella e quindi fare clic su **proprietà**.
3. Nel **sicurezza** scheda, fare clic su **Edit**e quindi fare clic su **Add**.
4. Fare clic su **posizioni**. Nel **posizioni** finestra di dialogo, selezionare il server locale e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. Nel **Seleziona utenti o gruppi** finestra di dialogo, digitare **IIS\_IUSRS**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.
6. Nel <strong>le autorizzazioni per</strong><em>[NomeCartella]</em> la finestra di dialogo, si noti che il nuovo gruppo è stato assegnato il <strong>lettura &amp; eseguire</strong>, <strong>visualizzazione contenuto cartella contenuto</strong>, e <strong>lettura</strong> le autorizzazioni per impostazione predefinita. Lasciare invariata e fare clic su <strong>OK</strong>.
7. Fare clic su <strong>OK</strong> per chiudere la <em>[NomeCartella]</em><strong>proprietà</strong> nella finestra di dialogo.

Come un'attività finale, è necessario concedere le autorizzazioni appropriate per l'utente non amministratore, le cui credenziali si userà per distribuire il contenuto. L'utente richiede le autorizzazioni per distribuire il contenuto in remoto al sito Web.

**Configurare le autorizzazioni di siti Web IIS per un utente senza privilegi di amministratore del dominio**

1. In Gestione IIS nel **connessioni** riquadro, fare clic sul nodo del sito Web (ad esempio, **DemoSite**), scegliere **Distribuisci**e quindi fare clic su **configurare Web Pubblicazione con distribuzione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. Nel **configurare la distribuzione di pubblicazione sul Web** a destra della finestra di dialogo il **selezionare un utente a cui concedere le autorizzazioni di pubblicazione** elenco, fare clic sul pulsante con puntini di sospensione.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. Nel **utente consentono** finestra di dialogo, digitare il nome utente e dominio dell'account da usare per distribuire il contenuto e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. Nel **configurare la distribuzione di pubblicazione sul Web** della finestra di dialogo fare clic su **installazione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Questa operazione esegue due funzioni principali in un unico passaggio. In primo luogo, concede all'utente l'autorizzazione per modificare il sito Web in modalità remota tramite servizio di gestione Web, secondo le regole di delega che sono stati esaminati nella sezione precedente. In secondo luogo, concede il controllo completo all'utente della cartella di origine per il sito Web, che consente all'utente di aggiungere, modificare e impostare le autorizzazioni per il contenuto del sito Web.
5. Nel **configurare la distribuzione di pubblicazione sul Web** della finestra di dialogo fare clic su **Chiudi**.

## <a name="configure-firewall-exceptions"></a>Configurare eccezioni del Firewall

Per impostazione predefinita, il servizio di gestione Web di IIS è in ascolto sulla porta TCP 8172. Se Windows Firewall è abilitato nel server web, è necessario creare una nuova regola in ingresso per consentire il traffico TCP sulla porta 8172 (tutto il traffico in uscita è consentito per impostazione predefinita in Windows Firewall). Se si usa un firewall di terze parti, è necessario creare regole per consentire il traffico.

| Direzione | Dalla porta | Eseguire il porting | Tipo di porta |
| --- | --- | --- | --- |
| Connessioni in entrata | Qualsiasi | 8172 | TCP |
| In uscita | 8172 | Qualsiasi | TCP |
  

Per altre informazioni sulla configurazione di regole Firewall di Windows, vedere [configurare le regole del Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Per i firewall di terze parti, consultare la documentazione del prodotto.

## <a name="conclusion"></a>Conclusione

Il server web dovrebbe ora essere pronto per accettare le distribuzioni remote al gestore di distribuzione Web tramite il servizio di gestione Web. Prima di tentare di distribuire un'applicazione web al server, è possibile controllare questi punti importanti:

- È stato abilitato l'autenticazione di base a livello di server in IIS?
- Le connessioni remote al servizio Web di gestione è stato abilitato?
- Il servizio di gestione Web è stato avviato?
- Esistono Gestione regole del servizio delega posto?
- L'identità del pool ha accesso in lettura alla cartella di origine per il sito Web?
- L'account utente senza privilegi di amministratore ha le autorizzazioni a livello di sito in IIS?
- Il firewall consente le connessioni in ingresso per il server sulla porta TCP 8172?

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni su come configurare i file di progetto di Microsoft Build Engine (MSBuild) personalizzati per distribuire i pacchetti web per il gestore di distribuzione Web, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Precedente](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
