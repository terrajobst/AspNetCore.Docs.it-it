---
uid: web-api/overview/security/external-authentication-services
title: Servizi di autenticazione esterno con ASP.NET Web API (c#) | Documenti Microsoft
author: rmcmurray
description: Viene descritto l'utilizzo di servizi di autenticazione esterni nell'API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 406a85db7055910cb7a4e15fec8ef68dff5a19dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Servizi di autenticazione esterno con ASP.NET Web API (c#)
====================
da [Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 e ASP.NET 4.5.1 espandere le opzioni di sicurezza per [Single Page Applications](../../../single-page-application/index.md) (SPA) e [API Web](../../index.md) servizi per l'integrazione con servizi di autenticazione esterno, che includono numerosi Servizi di autenticazione di social networking e OAuth/OpenID: Accounts Microsoft, Twitter, Facebook e Google.

### <a name="in-this-walkthrough"></a>In questa procedura dettagliata

- [Utilizzo di servizi di autenticazione esterni](#USING)
- [Creazione dell'applicazione Web di esempio](#SAMPLE)
- [Abilitazione dell'autenticazione di Facebook](#FACEBOOK)
- [Abilitazione dell'autenticazione di Google](#GOOGLE)
- [Abilitazione dell'autenticazione di Microsoft](#MICROSOFT)
- [Abilitazione dell'autenticazione di Twitter](#TWITTER)
- [Informazioni aggiuntive](#MOREINFO)

    - [La combinazione di servizi di autenticazione esterni](#COMBINE)
    - [Configurazione di IIS Express per utilizzare un nome di dominio completo](#FQDN)
    - [Come ottenere le impostazioni dell'applicazione per l'autenticazione di Microsoft](#OBTAIN)
    - [Facoltativo: Disabilitare la registrazione locale](#DISABLE)

### <a name="prerequisites"></a>Prerequisiti

Per seguire gli esempi in questa procedura dettagliata, è necessario disporre di quanto segue:

- Visual Studio 2013
- Un account per almeno uno dei seguenti servizi di autenticazione esterni:

    - Un account utente di Google
    - Un account sviluppatore con l'identificatore dell'applicazione e una chiave privata per uno dei servizi di autenticazione di social networking seguenti:

        - Gli account Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Utilizzo di servizi di autenticazione esterni

Il numero elevato di servizi di autenticazione esterno che sono attualmente disponibili per la Guida agli sviluppatori web per ridurre lo sviluppo di tempo durante la creazione di nuove applicazioni web. Gli utenti Web in genere dispongono di diversi account esistenti per servizi web più comuni e i siti Web di social networking, pertanto quando si implementa un'applicazione web l'autenticazione dei servizi da un servizio web esterno o il sito Web di social networking, Salva la fase di sviluppo che spesa la creazione di un'implementazione di autenticazione. Utilizzo di un servizio di autenticazione esterno Salva gli utenti finali di dover creare un altro account per l'applicazione web e di dover ricordare un altro nome utente e password.

In passato, gli sviluppatori hanno dovuto disponibili due opzioni: creare la propria implementazione di autenticazione, o informazioni su come integrare un servizio di autenticazione esterni nelle applicazioni. Livello di base, il diagramma seguente illustra un flusso di richiesta semplice di un agente utente (browser web) che richiede informazioni da un'applicazione web è configurata per utilizzare un servizio di autenticazione esterni:

[![](external-authentication-services/_static/image2.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image1.png)

Nel diagramma precedente, l'agente utente (o un browser web in questo esempio) effettua una richiesta a un'applicazione web, che reindirizza il browser web a un servizio di autenticazione esterno. L'agente utente invia le credenziali per il servizio di autenticazione esterno, e se l'agente utente è autenticato, il servizio di autenticazione esterno reindirizzerà l'agente utente all'applicazione web originale con un tipo di token che il agente utente invierà all'applicazione web. L'applicazione web utilizzerà il token per verificare che l'agente utente è stato autenticato dal servizio di autenticazione esterno e l'applicazione web può usare il token per raccogliere ulteriori informazioni sull'agente utente. Dopo l'applicazione ha terminato l'elaborazione di informazioni dell'agente utente, l'applicazione web verrà restituito la risposta appropriata per l'agente utente in base alle proprie impostazioni di autorizzazione.

Nel secondo esempio, l'agente utente negozia con l'applicazione web e server di autorizzazione esterni e l'applicazione web esegue ulteriori comunicazioni con il server di autorizzazione esterni per recuperare informazioni aggiuntive sull'utente agente:

[![](external-authentication-services/_static/image4.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image3.png)

Visual Studio 2013 e ASP.NET 4.5.1 semplificano l'integrazione con servizi di autenticazione esterni per gli sviluppatori fornendo integrazione incorporata per i servizi di autenticazione seguenti:

- Facebook
- Google
- Microsoft Accounts (account di Windows Live ID)
- Twitter

Negli esempi inclusi in questa procedura dettagliata verranno illustrato come configurare ognuno dei servizi di autenticazione esterni supportati tramite il nuovo modello di applicazione Web ASP.NET che viene fornito con Visual Studio 2013.

> [!NOTE]
> Se necessario, si potrebbe essere necessario aggiungere il nome di dominio completo per le impostazioni per il servizio di autenticazione esterno. Questo requisito è in base ai vincoli di sicurezza per alcuni servizi di autenticazione esterno che richiedono il FQDN nelle impostazioni dell'applicazione in modo da corrispondere al nome FQDN usato dai client di. (I passaggi per questo variano notevolmente per ogni servizio di autenticazione esterno, è necessario consultare la documentazione per ogni servizio di autenticazione esterno per vedere se questa operazione è necessaria e su come configurare queste impostazioni). Se è necessario configurare IIS Express per l'utilizzo di un FQDN per questo ambiente di testing, vedere il [la configurazione di IIS Express per utilizzare un nome di dominio completo](#FQDN) sezione più avanti in questa procedura dettagliata.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Creazione di un'applicazione Web di esempio

La procedura seguente verrà avviata la creazione di un'applicazione di esempio utilizzando il modello di applicazione Web ASP.NET e si utilizzerà questa applicazione di esempio per ognuno dei servizi di autenticazione esterno più avanti in questa procedura dettagliata.

Avviare Visual Studio 2013 selezionare **nuovo progetto** dalla pagina iniziale. O dal **File** dal menu **New** e quindi **progetto**.

[![](external-authentication-services/_static/image6.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image5.png)

Quando il **nuovo progetto** è visualizzata la finestra di dialogo, selezionare **installato** **modelli** espandere **Visual c#**. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET**. Immettere un nome per il progetto e fare clic su **OK**.

[![](external-authentication-services/_static/image8.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image7.png)

Quando il **nuovo progetto ASP.NET** è visualizzata, scegliere il **SPA** modello e fare clic su **Crea progetto**.

[![](external-authentication-services/_static/image10.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image9.png)

Attesa di Visual Studio 2013 consente di creare il progetto.

[![](external-authentication-services/_static/image12.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image11.png)

Quando Visual Studio 2013 ha terminato la creazione del progetto, aprire il *Startup.Auth.cs* file a cui si trova il **App\_avviare** cartella.

[![](external-authentication-services/_static/image14.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image13.png)

Quando si crea il progetto, nessuno dei servizi di autenticazione esterni sono abilitati *Startup.Auth.cs* file; le operazioni seguenti viene illustrato ciò che potrebbe essere simile al codice, con le sezioni evidenziate per stabilire dove abiliti un servizio di autenticazione esterno ed eventuali impostazioni rilevanti per usare l'autenticazione di Microsoft Accounts, Twitter, Facebook o Google con l'applicazione ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Quando si preme F5 per compilare ed eseguire il debug dell'applicazione web, visualizzerà una schermata di accesso in cui si noterà che non sono stati definiti servizi di autenticazione esterni.

[![](external-authentication-services/_static/image16.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image15.png)

Nelle sezioni seguenti, si apprenderà come abilitare ognuno dei servizi di autenticazione esterno che vengono forniti con ASP.NET in Visual Studio 2013.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Abilitazione dell'autenticazione di Facebook

Utilizzano Facebook autenticazione è necessario creare un account sviluppatore di Facebook e il progetto richiederà un ID applicazione e una chiave privata da Facebook per il funzionamento. Per informazioni sulla creazione di un account sviluppatore di Facebook e ottenere l'ID dell'applicazione e una chiave privata, vedere [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Una volta ottenuta l'ID dell'applicazione e una chiave privata, attenersi alla procedura seguente per abilitare l'autenticazione di Facebook per l'applicazione web:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image18.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image17.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Rimuovere il &quot; // &quot; caratteri da rimuovere il commento le righe di codice evidenziate e quindi aggiungere l'ID dell'applicazione e una chiave privata. Dopo aver aggiunto tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Facebook è stato definito come un servizio di autenticazione esterni:

    [![](external-authentication-services/_static/image20.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image19.png)
5. Quando si sceglie il **Facebook** pulsante, il browser verrà reindirizzato alla pagina di accesso Facebook:

    [![](external-authentication-services/_static/image22.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image21.png)
6. Dopo aver immesso le credenziali di Facebook e fare clic su **Accedi**, web browser verrà reindirizzato all'applicazione web, che richiederà il **nome utente** che si desidera associare il Account Facebook:

    [![](external-authentication-services/_static/image24.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image23.png)
7. Dopo aver immesso il nome utente e fa clic su di **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** per l'account Facebook:

    [![](external-authentication-services/_static/image26.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Abilitazione dell'autenticazione di Google

Google è di gran lunga il più semplice di servizi di autenticazione esterni da abilitare perché non richiede un account sviluppatore, né richiede informazioni aggiuntive, ad esempio l'ID dell'applicazione o una chiave privata come gli altri servizi di autenticazione esterno necessitano.

Per abilitare l'autenticazione di Google per l'applicazione web, attenersi alla procedura seguente:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image28.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image27.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Rimuovere il &quot; // &quot; caratteri da rimuovere il commento la riga di codice evidenziata e quindi ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Google è stato definito come un servizio di autenticazione esterni:

    [![](external-authentication-services/_static/image30.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image29.png)
5. Quando si sceglie il **Google** pulsante, il browser verrà reindirizzato alla pagina di accesso di Google:

    [![](external-authentication-services/_static/image32.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image31.png)
6. Dopo aver immesso le credenziali di Google e fare clic su **Accedi**, Google verrà chiesto di verificare che l'applicazione web disponga delle autorizzazioni per accedere all'account di Google:

    [![](external-authentication-services/_static/image34.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image33.png)
7. Quando fa clic su **Accept**, web browser verrà reindirizzato all'applicazione web, che richiederà il **nome utente** che si desidera associare all'account di Google:

    [![](external-authentication-services/_static/image36.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image35.png)
8. Dopo aver immesso il nome utente e fa clic su di **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** per l'account di Google:

    [![](external-authentication-services/_static/image38.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Abilitazione dell'autenticazione di Microsoft

L'autenticazione di Microsoft è necessario creare un account sviluppatore, e richiede un ID client e il segreto client per il funzionamento. Per informazioni sulla creazione di un account sviluppatore di Microsoft e ottenere l'ID client e il segreto client, vedere [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Una volta ottenuta la chiave di consumer e un segreto del cliente, attenersi alla procedura seguente per abilitare l'autenticazione di Microsoft per l'applicazione web:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image40.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image39.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Rimuovere il &quot; // &quot; caratteri per rimuovere il commento le righe di codice evidenziate e quindi aggiungere l'ID client e il segreto client. Dopo aver aggiunto tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Microsoft sia stato definito come un servizio di autenticazione esterni:

    [![](external-authentication-services/_static/image42.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image41.png)
5. Quando si sceglie il **Microsoft** pulsante, il browser verrà reindirizzato alla pagina di accesso di Microsoft:

    [![](external-authentication-services/_static/image44.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image43.png)
6. Dopo aver immesso le credenziali di Microsoft e fare clic su **Accedi**, verrà richiesto di verificare che l'applicazione web disponga delle autorizzazioni per accedere all'account Microsoft:

    [![](external-authentication-services/_static/image46.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image45.png)
7. Quando fa clic su **Sì**, web browser verrà reindirizzato all'applicazione web, che richiederà il **nome utente** che si desidera associare all'account Microsoft:

    [![](external-authentication-services/_static/image48.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image47.png)
8. Dopo aver immesso il nome utente e fa clic su di **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** del tuo account Microsoft:

    [![](external-authentication-services/_static/image50.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Abilitazione dell'autenticazione di Twitter

L'autenticazione è necessario creare un account sviluppatore, e richiede una chiave utente e il segreto del cliente per il funzionamento di Twitter. Per informazioni sulla creazione di un account sviluppatore di Twitter e ottenere il codice del cliente e il segreto del cliente, vedere [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Una volta ottenuta la chiave di consumer e un segreto del cliente, attenersi alla procedura seguente per abilitare l'autenticazione di Twitter per l'applicazione web:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image52.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image51.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Rimuovere il &quot; // &quot; caratteri da rimuovere il commento le righe di codice evidenziate e quindi aggiungere il codice e il segreto del cliente. Dopo aver aggiunto tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Twitter sia stato definito come un servizio di autenticazione esterni:

    [![](external-authentication-services/_static/image54.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image53.png)
5. Quando si sceglie il **Twitter** pulsante, il browser verrà reindirizzato alla pagina di accesso Twitter:

    [![](external-authentication-services/_static/image56.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image55.png)
6. Dopo aver immesso le credenziali di Twitter e fare clic su **Authorize app**, web browser verrà reindirizzato all'applicazione web, che richiederà il **nome utente** che si desidera associare a l'account Twitter:

    [![](external-authentication-services/_static/image58.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image57.png)
7. Dopo aver immesso il nome utente e fa clic su di **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** per l'account Twitter:

    [![](external-authentication-services/_static/image60.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Informazioni aggiuntive

Per ulteriori informazioni sulla creazione di applicazioni che utilizzano OAuth e OpenID, vedere i seguenti URL:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>La combinazione di servizi di autenticazione esterni

Per una maggiore flessibilità, è possibile definire più servizi di autenticazione esterno allo stesso tempo - web in questo modo gli utenti dell'applicazione di utilizzare un account da uno dei servizi di autenticazione esterno abilitato:

[![](external-authentication-services/_static/image62.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Configurazione di IIS Express per utilizzare un nome di dominio completo

Alcuni provider di autenticazione esterno non supportano il test dell'applicazione utilizzando un indirizzo HTTP come `http://localhost:port/`. Per risolvere questo problema, è possibile aggiungere un mapping statico di completamente dominio nome completo (FQDN) per il file host e configurare le opzioni di progetto in Visual Studio 2013, usare il FQDN per il test e il debug. A tale scopo, attenersi alla procedura seguente:

- Aggiungere un nome FQDN statico mapping nel file HOSTS:

  1. Aprire un prompt dei comandi con privilegi elevati in Windows.
  2. Digitare il comando seguente:

      <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
  3. Aggiungere una voce simile alla seguente al file HOSTS:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Salvare e chiudere il file HOSTS.

- Configurare il progetto di Visual Studio per usare il FQDN:

  1. Quando il progetto è aperto in Visual Studio 2013, fare clic su di **progetto** menu, quindi selezionare le proprietà del progetto. Ad esempio, è possibile selezionare **WebApplication1 proprietà**.
  2. Selezionare il **Web** scheda.
  3. Immettere il nome FQDN per il <strong>Url progetto</strong>. Ad esempio, immettere <kbd> <http://www.wingtiptoys.com> </kbd> se che era il mapping del nome FQDN che è stato aggiunto al file HOSTS.

- Configurare IIS Express per usare il FQDN per l'applicazione:

    1. Aprire un prompt dei comandi con privilegi elevati in Windows.
    2. Digitare il comando seguente per passare alla cartella IIS Express:

        <kbd>CD /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Digitare il comando seguente per aggiungere il nome FQDN nell'applicazione:

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  Dove **WebApplication1** è il nome del progetto e **bindingInformation** contiene il numero di porta e il nome di dominio completo che si desidera utilizzare per il test.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Come ottenere le impostazioni dell'applicazione per l'autenticazione di Microsoft

Il collegamento di un'applicazione a Windows Live per Microsoft Authentication è un processo semplice. Se un'applicazione a Windows Live non è già collegato, è possibile utilizzare la procedura seguente:

1. Passare a [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) e immettere il nome dell'account Microsoft e la password, quando richiesto, quindi fare clic su **Accedi**:

    [![](external-authentication-services/_static/image64.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image63.png)
2. Immettere il nome e la lingua dell'applicazione quando viene richiesto e quindi fare clic su **accettare**:

    [![](external-authentication-services/_static/image66.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image65.png)
3. Nel **impostazioni API** pagina per l'applicazione, immettere il dominio di reindirizzamento per l'applicazione e copia il **ID Client** e **segreto Client** per il progetto, quindi Fare clic su **salvare**:

    [![](external-authentication-services/_static/image68.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Facoltativo: Disabilitare la registrazione locale

La funzionalità di registrazione locale corrente di ASP.NET non impedisce la creazione di account; membro di programmi automatizzati (componenti) ad esempio, utilizzando una tecnologia di convalida e di prevenzione bot come [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Per questo motivo, è necessario rimuovere il collegamento di modulo e la registrazione di account di accesso locale nella pagina di accesso. A tale scopo, aprire il * \_cshtml* pagina nel progetto e quindi impostare come commento le righe per il pannello di accesso locale e il collegamento di registrazione. Nella pagina risultante dovrebbe essere come nell'esempio di codice seguente:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Una volta il pannello di accesso locale e il collegamento di registrazione sono state disabilitate, la pagina di accesso verrà visualizzate solo i provider di autenticazione esterno che è stata abilitata:

[![](external-authentication-services/_static/image70.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image69.png)
