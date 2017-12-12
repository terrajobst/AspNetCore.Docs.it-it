---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Creazione di una Server Farm con Web Farm Framework | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come utilizzare la Web Farm Framework (WFF) 2.0 per creare e configurare una web farm di server da una raccolta di server.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 2458abc863a83364f90fc9d6edaace897c23b4c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Creazione di una Server Farm con Web Farm Framework
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come utilizzare la Web Farm Framework (WFF) 2.0 per creare e configurare una web farm di server da una raccolta di server.


WFF consente di sincronizzare i prodotti della piattaforma web e i componenti, applicazioni web, siti Web e le impostazioni di configurazione tra più server web con bilanciamento del carico. Negli scenari in cui è necessario più di un server web, ad esempio gli ambienti di gestione temporanea e produzione, ciò consente di semplificare notevolmente il processo di distribuzione e configurazione. È possibile distribuire un'applicazione web in un server singolo & #x 2014; il *server primario*& #x 2014; e che verranno WFF consente di replicare automaticamente l'applicazione web su tutti gli altri server web nella server farm.

## <a name="understanding-the-web-farm-framework"></a>Informazioni sulle Web Farm Framework

È possibile utilizzare 2.0 WFF per effettuare il provisioning, gestire e distribuire il contenuto a un gruppo di server web. Una distribuzione WFF è costituita da tre ruoli principali del server:

- Il *server controller*. Questo server viene utilizzato per creare e configurare la farm di server WFF. Il server controller gestisce la sincronizzazione di componenti della piattaforma web, le impostazioni di configurazione e le applicazioni tra i server web in una server farm. Si installa WFF 2.0 nel server di controller e il server controller verrà a sua volta installato l'agente WFF in ogni server in una server farm. Il server del controller non appartiene a livello concettuale di alcuna server farm WFF e un server singolo controller può gestire più server farm. In questo scenario, utilizzare un singolo server di controller WFF per creare e gestire la farm di server di gestione temporanea e la server farm di produzione.
- Il *server primario*. Ogni server farm WFF include un singolo server primario. Quando si installino i componenti della piattaforma web o si distribuiscono applicazioni per il server primario, il WFF consente di sincronizzare le modifiche a tutti gli altri server nella server farm.
- Il *server secondario*. Ogni server farm WFF include uno o più server secondari. Le modifiche apportate al server primario vengono replicate in ogni server secondario all'interno della farm di server.

Viene illustrato come questi ruoli del server relative agli ambienti di gestione temporanea e produzione di Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

In questo scenario, l'ambiente di gestione temporanea e l'ambiente di produzione sono entrambi configurati come server farm di WFF. Un singolo server di controller WFF gestisce entrambe le farm. All'interno di ogni server farm, tutte le modifiche al server primario vengono replicate in ogni server secondario.

Prima di iniziare a configurare gli ambienti di gestione temporanea e produzione, è consigliabile leggere gli articoli per acquisire familiarità con i concetti principali di WFF 2.0:

- [Panoramica di Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configurazione di una Server Farm con Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisiti di sistema e della piattaforma per il Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Panoramica di Task

Per completare le attività e procedure dettagliate in questo argomento, è necessario almeno tre server e #x 2014; un controller WFF, server web primario uno per la server farm e uno o più server web secondario per la server farm. È possibile aggiungere più server secondari a una server farm WFF in qualsiasi momento. In generale, per creare e configurare una server farm WFF per l'ambiente di produzione o gestione temporanea, che è necessario:

- Creare un server di controller mediante l'installazione di Internet Information Services (IIS) 7.5 e WFF 2.0.
- Preparare i server primari e secondari, creare un account di amministratore comuni e configurando le eccezioni del firewall.
- Configurare la farm di server tramite Gestione IIS nel server di controller.
- Configurare il bilanciamento del carico utilizzando IIS Application Request Routing (ARR) o una tecnologia di bilanciamento del carico alternativa.

Le attività e procedure dettagliate in questo argomento si presuppongono che si sta iniziando con le compilazioni di server pulito in esecuzione Windows Server 2008 R2. Prima di iniziare, per ogni server, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili vengono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e registrazione](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Creare il Server di Controller WFF

Per creare un server di controller WFF, è necessario installare IIS 7 o versioni successive sia WFF 2.0 o versione successiva. Dietro le quinte, WFF utilizza lo strumento di distribuzione Web di IIS (distribuzione Web) 2. x per sincronizzare i server nella farm. Se si utilizza l'installazione guidata piattaforma Web per installare WFF, il programma di installazione verrà automaticamente scaricare e installare distribuzione Web per l'utente.

**Per creare il server di controller WFF**

1. Scaricare e installare il [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9739157).
2. Nella parte superiore del **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
3. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Server**.
4. Nel **configurazione consigliata di IIS 7** di riga, fare clic su **Aggiungi**.
5. Nel **di Web Farm Framework 2.** *x* di riga, fare clic su **Aggiungi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Fare clic su **Installa**. Si noti che l'installazione guidata piattaforma Web è aggiunto all'elenco installazione lo strumento di distribuzione Web, insieme a varie altre dipendenze.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Esaminare le condizioni di licenza e, se accettano le condizioni, fare clic su **accetto**.
8. Una volta completato l'installazione, fare clic su **fine**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

## <a name="configure-the-primary-and-secondary-servers"></a>Configurare il server primario e secondario

Prima di creare una server farm WFF, è necessario completare alcune attività di preparazione nei server web che costituiranno farm:

- Aggiungere le eccezioni del firewall per consentire il **funzionalità di base rete**, **amministrazione remota**, e **condivisione File e stampanti** per comunicare con il server di controller WFF .
- Creare un account di dominio (ad esempio, **FABRIKAM\stagingfarm**) in Active Directory e aggiungerlo al gruppo administrators locale in ogni server. Si utilizzerà questo account come account di amministratore della server farm quando si crea la server farm.

Per ulteriori informazioni su come configurare le eccezioni del firewall in Windows Firewall, vedere [requisiti di sistema e piattaforma di Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805128). Per altri sistemi firewall, consultare la documentazione del prodotto.

È possibile utilizzare la procedura seguente per aggiungere un account di dominio al gruppo administrators locale in Windows Server 2008 R2. È necessario eseguire questa procedura su ogni server che si desidera aggiungere alla farm di server & #x 2014; in altre parole, aggiungere lo stesso account di dominio al gruppo administrators locale nel server primario e in ciascun server secondario.

**Per aggiungere un account di dominio al gruppo administrators locale**

1. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Server Manager**.
2. Nel **Server Manager** finestra, nel riquadro di visualizzazione albero, espandere **configurazione**, espandere **utenti e gruppi locali**, quindi fare clic su **gruppi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. Nel **gruppi** riquadro, fare doppio clic su **amministratori**.
4. Nel **proprietà amministratori** la finestra di dialogo, fare clic su **Aggiungi**.
5. Nel **Seleziona utenti, computer, account di servizio o gruppi** la finestra di dialogo digitare (o selezionare) per l'account di dominio (ad esempio, **FABRIKAM\stagingfarm**), quindi fare clic su **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. Nel **proprietà amministratori** la finestra di dialogo, fare clic su **OK**.

I server sono ora pronti per l'aggiunta a una server farm. Nel caso il server primario, è possibile configurare il server per soddisfare i requisiti dell'applicazione, prima o dopo aver creato la farm di server & #x 2014; in entrambi i casi, il WFF verranno sincronizzati i server distribuendo i prodotti stesso, componenti, o configurazione dei server secondari. Per ragioni di semplicità, in questa esercitazione si presuppone che si configurerà il server primario quando viene creata la server farm.

## <a name="create-the-wff-server-farm"></a>Creare la Farm di Server WFF

A questo punto, tutti i server pronti essere aggiunto a una server farm WFF:

- WFF è stato installato nel server di controller.
- Le eccezioni del firewall configurati nei server web primario e secondario.
- Hai aggiunto un account di dominio al gruppo administrators locale nei server web primario e secondario.

Il passaggio successivo consiste nel creare la server farm in WFF. È possibile farlo da Gestione IIS nel server di controller WFF.

**Per creare una server farm WFF**

1. Sul server, controller WFF sul **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)**.
2. Nel **connessioni** riquadro espandere il nodo del server locale, fare doppio clic su **Server farm**, quindi fare clic su **crea Server Farm**.
3. Nel **crea Server Farm** finestra di dialogo, digitare un nome significativo per la server farm (ad esempio, **Farm di gestione temporanea**), quindi selezionare **farm di server eseguire il provisioning**.
4. Digitare il nome utente e la password dell'account di dominio che è stato aggiunto al gruppo administrators locale in ogni server.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Scegliere **Avanti**.
6. Nel **Aggiungi server** pagina, digitare il nome di dominio completo (FQDN) del server primario, selezionare **Server primario**, quindi fare clic su **Aggiungi**.
7. A questo punto, WFF tenterà di contattare il server primario utilizzando le credenziali specificate. Se la connessione ha esito positivo, il server primario verrà aggiunto alla tabella nel **Aggiungi server** pagina.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > È possibile aver notato che **Server è disponibile per il bilanciamento del carico** è selezionata per impostazione predefinita. WFF Usa il modulo IIS ARR per implementare il bilanciamento del carico e quindi distribuire le richieste tra i server web nella server farm. Nella maggior parte degli scenari, è solo deselezionare il **Server è disponibile per il bilanciamento del carico** opzione se si desidera utilizzare un terza parte bilanciamento carico di soluzione alternativa.
8. Nel **Aggiungi server** pagina, digitare il nome FQDN del server secondario prima e quindi fare clic su **Aggiungi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Ripetere il passaggio 7 per tutti i server secondari aggiuntivi nella farm e quindi fare clic su **fine**.

La server farm WFF è ora installato e in esecuzione. Tutti i prodotti della piattaforma web o componenti da installare nel server primario e le applicazioni web o contenuto distribuito al server primario, eseguire automaticamente il provisioning in tutti i server secondari.

WFF è un argomento ampio e complesso, e sono disponibili ulteriori informazioni sul [Microsoft Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805129) sito Web. Per il momento, tuttavia, ci sono due aree di funzionalità che è necessario tenere in considerazione:

- *Provisioning di applicazioni* è il processo che consente di replicare il contenuto dal server primario, ad esempio applicazioni web e le impostazioni di configurazione, tra tutti i server secondari nella server farm. Ad esempio, se si distribuisce la soluzione di esempio di gestione di contatto per il server di gestione temporanea principale, il processo di provisioning dell'applicazione WFF distribuirà questa soluzione per tutti i server di gestione temporanea secondari. Per impostazione predefinita, il processo di provisioning dell'applicazione viene eseguito ogni 30 secondi.
- *Il provisioning della piattaforma* è il processo che consente di sincronizzare i prodotti della piattaforma web e ai componenti del server primario di tutti i server secondari nella server farm. Ad esempio, se si installa ASP.NET MVC 3 sul server primario di gestione temporanea, il processo di provisioning della piattaforma utilizzerà l'installazione guidata piattaforma Web per installare ASP.NET MVC 3 in tutti i server di gestione temporanea secondari. Per impostazione predefinita, il processo di provisioning della piattaforma viene eseguito ogni cinque minuti.

È possibile gestire un'applicazione di base e le impostazioni di provisioning della piattaforma da Gestione IIS sul server del controller WFF.

**Esplorare l'applicazione e le impostazioni di provisioning della piattaforma**

1. In Gestione IIS, nel **connessioni** riquadro, selezionare la server farm.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. Nel **Server Farm** riquadro, fare doppio clic su **Provisioning delle applicazioni**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Come si può notare, la farm di server è attualmente configurata per sincronizzare le impostazioni di configurazione e il contenuto web tra il server primario e i server secondari ogni 30 secondi.
4. Fare clic su **nuovamente**, quindi fare doppio clic su **il Provisioning della piattaforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Come si può notare, la farm di server è attualmente configurata per sincronizzare i prodotti della piattaforma web e dei componenti tra il server primario e i server secondari ogni cinque minuti.
6. Fare clic su **nuovamente**.
7. Per forzare la server farm per sincronizzare immediatamente, i prodotti della piattaforma web nel **azioni** riquadro, fare clic su **provisioning piattaforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Il provisioning della piattaforma potrebbe richiedere alcuni minuti. Il processo di installazione viene eseguito in background nei server secondari nella server farm.
8. Dopo avere un tempo sufficiente per completare il processo di provisioning, è possibile verificare che i prodotti e componenti che è stato aggiunto al server primario siano stati replicati ora sui server secondari. Ad esempio, è possibile accedere a un server secondario e utilizzare il **Server Manager** per verificare che sia stato installato il ruolo server web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. È inoltre possibile verificare l'elenco dei programmi installati per verificare che siano stati aggiunti i vari componenti della piattaforma web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurare il bilanciamento del carico

Quando si crea una web farm, è necessario configurare una forma di bilanciamento del carico per distribuire le richieste HTTP tra i server web. Potrebbe trattarsi di bilanciamento del carico, IIS ARR, carico di rete di Windows Server 2008 o un carico di terze parti basati su hardware o software soluzione di bilanciamento del carico.

WFF è progettato per l'integrazione con IIS ARR. Per sfruttare l'integrazione, è necessario installare il modulo ARR sul server di controller WFF. Quindi indirizzare il traffico web per il server del controller, in genere configurando i record di sistema DNS (Domain Name). Il server controller distribuirà quindi le richieste in ingresso tra i server nella farm, basati sulla disponibilità del server e vari altri criteri.

> [!NOTE]
> Non è necessario usare ARR con WFF; è possibile configurare WFF per funzionare con soluzioni di bilanciamento carico di terze parti. Per ulteriori informazioni, vedere [Panoramica di Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805126).


Bilanciamento del carico con ARR è un argomento complesso, più che non rientra nell'ambito di questa esercitazione. Tuttavia, è possibile utilizzare la procedura seguente per installare il modulo ARR e iniziare a usare il bilanciamento del carico.

**Per configurare il bilanciamento del carico sul server di controller WFF**

1. Nel server controller WFF, avviare l'installazione guidata piattaforma Web.
2. Nella parte superiore del **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
3. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Server**.
4. Nel **Application Request Routing 2.5** di riga, fare clic su **Aggiungi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Fare clic su **installare**e quindi seguire le istruzioni di **installazione guidata piattaforma Web** finestra.
6. Una volta completato l'installazione, avviare Gestione IIS e il **connessioni** riquadro, scegliere il nodo della server farm. Si noti che sono state aggiunte numerose nuove icone per il **Server Farm** riquadro.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. Nel **Server Farm** riquadro, fare doppio clic su **bilanciamento del carico**.
8. Nel **Load Balance** riquadro, selezionare un carico bilanciare algoritmo (ad esempio, **richiesta almeno corrente**).

    > [!NOTE]
    > Per ulteriori informazioni sul bilanciamento del carico algoritmi e altre impostazioni di configurazione, vedere [modulo di Routing richiesta applicazione](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. Nel **azioni** riquadro, fare clic su **applica**.

È stato configurato per i server nella server farm di bilanciamento del carico di base. Se indirizzare tutto il traffico per la farm di web server del controller, le richieste verranno distribuite tra i server della farm in base alla disponibilità e l'algoritmo di bilanciamento del carico che è stato selezionato.

Per ulteriori informazioni su come configurare il bilanciamento del carico con ARR, vedere [modulo di Routing richiesta applicazione](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitoraggio della Server Farm

È possibile monitorare l'integrità della server farm in qualsiasi momento tramite Gestione IIS nel server di controller. Nel **connessioni** riquadro, espandere server farm e quindi fare clic su **server**. Riquadro centrale visualizzerà un riepilogo di ogni server nella farm insieme a un log di traccia delle attività recenti.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusione

La server farm WFF dovrebbe ora essere in esecuzione. È possibile configurare il server primario per il supporto indipendentemente dall'approccio scelto distribuzione & #x 2014, vedere la sezione di approfondimento per dettagli & #x 2014; e la configurazione verrà replicati in ciascun server secondario nella server farm.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni su tutti gli aspetti di configurazione e l'utilizzo di WFF, vedere il [Microsoft Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805129) sito Web.

>[!div class="step-by-step"]
[Precedente](configuring-a-database-server-for-web-deploy-publishing.md)
[Successivo](configuring-deployment-properties-for-a-target-environment.md)
