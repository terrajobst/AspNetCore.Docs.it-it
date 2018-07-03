---
uid: web-api/overview/security/external-authentication-services
title: Servizi di autenticazione esterno con l'API Web ASP.NET (c#) | Microsoft Docs
author: rmcmurray
description: Viene descritto l'utilizzo di servizi di autenticazione esterni nell'API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 9cfb8022670497f7552820e9df2ad5e0cf6f9f46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391405"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Servizi di autenticazione esterno con l'API Web ASP.NET (c#)
====================
da [Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 e ASP.NET 4.5.1 espandere le opzioni di sicurezza per [applicazioni a pagina singola](../../../single-page-application/index.md) (SPA) e [API Web](../../index.md) servizi per l'integrazione con servizi di autenticazione esterni, che includono numerosi OAuth/OpenID e servizi di autenticazione dei social media: Accounts Microsoft, Twitter, Facebook e Google.

### <a name="in-this-walkthrough"></a>In questa procedura dettagliata

- [Uso dei servizi di autenticazione esterno](#USING)
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
- Un account per almeno uno dei seguenti servizi di autenticazione esterno:

    - Un account utente di Google
    - Un account sviluppatore con l'identificatore dell'applicazione e la chiave privata per uno dei seguenti servizi di autenticazione dei social media:

        - Gli account Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Uso dei servizi di autenticazione esterno

La grande quantità di servizi di autenticazione esterni che sono attualmente disponibili per visualizzare la Guida agli sviluppatori web per ridurre lo sviluppo di tempo durante la creazione di nuove applicazioni web. Gli utenti Web in genere dispongono di diversi account esistenti per i servizi web più diffusi e siti Web di social media, pertanto quando si implementa un'applicazione web l'autenticazione di servizi da un servizio web esterno o un sito Web di social networking, Salva i tempi di sviluppo che spesa la creazione di un'implementazione di autenticazione. Usando un servizio di autenticazione esterni vengono salvati gli utenti finali di dover creare un altro account per l'applicazione web, nonché di dover ricordare un altro nome utente e password.

In passato, gli sviluppatori hanno dovuto due opzioni: creare la propria implementazione di autenticazione, o descrive come integrare un servizio di autenticazione esterni nelle proprie applicazioni. A livello di base, il diagramma seguente illustra un flusso semplice richiesta per un agente utente (un web browser) che richiede informazioni da un'applicazione web che è configurata per usare un servizio di autenticazione esterno:

[![](external-authentication-services/_static/image2.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image1.png)

Nel diagramma precedente, l'agente utente (o un web browser in questo esempio) esegue una richiesta a un'applicazione web, che reindirizza il browser web a un servizio di autenticazione esterni. L'agente utente invia le credenziali per il servizio di autenticazione esterni, e se l'agente utente è autenticato correttamente, il servizio di autenticazione esterno reindirizzerà l'agente utente all'applicazione web originale con qualche forma di token che la agente utente invierà all'applicazione web. L'applicazione web utilizzerà il token per verificare che l'agente utente è stato correttamente autenticato dal servizio di autenticazione esterni e l'applicazione web può usare il token per raccogliere ulteriori informazioni sull'agente utente. Dopo l'applicazione ha terminato l'elaborazione di informazioni dell'agente utente, l'applicazione web restituirà la risposta appropriata per l'agente utente in base alle relative impostazioni di autorizzazione.

In questo secondo esempio, l'agente utente negozia con l'applicazione web e server di autorizzazione esterni e l'applicazione web vengono eseguite ulteriori comunicazioni con il server di autorizzazione esterni per recuperare informazioni aggiuntive sull'utente agente:

[![](external-authentication-services/_static/image4.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image3.png)

Visual Studio 2013 e ASP.NET 4.5.1 semplificano l'integrazione con servizi di autenticazione esterni per gli sviluppatori fornendo integrazione predefinita per i servizi di autenticazione seguenti:

- Facebook
- Google
- Accounts Microsoft (account di Windows Live ID)
- Twitter

Gli esempi in questa procedura dettagliata verranno illustrato come configurare ognuno dei servizi di autenticazione esterni supportati tramite il nuovo modello applicazione Web ASP.NET viene fornito con Visual Studio 2013.

> [!NOTE]
> Se necessario, si potrebbe essere necessario aggiungere il nome di dominio completo per le impostazioni per il servizio di autenticazione esterni. Questo requisito si basa sui vincoli di sicurezza per alcuni servizi di autenticazione esterno che richiedono il FQDN nelle impostazioni dell'applicazione che corrisponde a quello utilizzato dal client. (I passaggi di questa procedura variano notevolmente per ogni servizio di autenticazione esterni, sarà necessario consultare la documentazione per ogni servizio di autenticazione esterni per vedere se questa operazione è necessaria e su come configurare queste impostazioni). Se è necessario configurare IIS Express per utilizzare un nome di dominio completo per questo ambiente di test, vedere la [configurazione di IIS Express per utilizzare un nome di dominio completo](#FQDN) sezione più avanti in questa procedura dettagliata.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Creazione di un'applicazione Web di esempio

La procedura seguente illustra la creazione di un'applicazione di esempio usando il modello di applicazione Web ASP.NET e si userà questa applicazione di esempio per ognuno dei servizi di autenticazione esterni più avanti in questa procedura dettagliata.

Avviare Visual Studio 2013 selezionare **nuovo progetto** dalla pagina iniziale. E viceversa, il **File** dal menu **New** e quindi **progetto**.

[![](external-authentication-services/_static/image6.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image5.png)

Quando la **nuovo progetto** verrà visualizzata la finestra di dialogo, selezionare **installati** **modelli** ed espandere **Visual c#**. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET**. Immettere un nome per il progetto e fare clic su **OK**.

[![](external-authentication-services/_static/image8.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image7.png)

Quando la **nuovo progetto ASP.NET** è visualizzato, selezionare la **SPA** modello, quindi scegliere **Crea progetto**.

[![](external-authentication-services/_static/image10.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image9.png)

Attesa di Visual Studio 2013 consente di creare il progetto.

[![](external-authentication-services/_static/image12.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image11.png)

Quando Visual Studio 2013 ha terminato la creazione del progetto, aprire il *Startup.Auth.cs* file di cui si trova il **App\_avviare** cartella.

[![](external-authentication-services/_static/image14.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image13.png)

Quando si crea innanzitutto il progetto, nessuno dei servizi di autenticazione esterni sono abilitati nella *Startup.Auth.cs* ; del file seguente viene illustrato ciò che potrebbe essere simile al codice, con le sezioni evidenziate per stabilire dove sarebbe necessario abilitare un servizio di autenticazione esterni e tutte le impostazioni rilevanti per usare l'autenticazione Accounts Microsoft, Twitter, Facebook o Google con l'applicazione ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Quando si preme F5 per compilare ed eseguire il debug dell'applicazione web, visualizzerà una schermata di accesso in cui noterete che non sono stati definiti servizi di autenticazione esterni.

[![](external-authentication-services/_static/image16.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image15.png)

Nelle sezioni seguenti, si apprenderà come consentire a ognuno dei servizi di autenticazione esterni forniti con ASP.NET core in Visual Studio 2013.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Abilitazione dell'autenticazione di Facebook

Con Facebook autenticazione richiede di creare un account per sviluppatore di Facebook e il progetto richiederà un ID applicazione e la chiave privata da Facebook per il funzionamento. Per informazioni sulla creazione di un account per sviluppatore di Facebook e ottenere l'ID applicazione e la chiave privata, vedere [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Una volta ottenuto l'ID applicazione e la chiave privata, usare la procedura seguente per abilitare l'autenticazione di Facebook per l'applicazione web:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image18.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image17.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Rimuovere il &quot; // &quot; caratteri per rimuovere il commento le righe evidenziate del codice e quindi aggiungere l'ID applicazione e la chiave privata. Dopo aver aggiunto tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Facebook è stata definita come un servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image20.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image19.png)
5. Quando si sceglie la **Facebook** pulsante, il browser verrà reindirizzato alla pagina di accesso di Facebook:

    [![](external-authentication-services/_static/image22.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image21.png)
6. Dopo aver immesso le credenziali di Facebook e fare clic su **accedere**, il browser web verrà reindirizzato all'applicazione web, che verranno richieste per il **nome utente** che si desidera associare il Account Facebook:

    [![](external-authentication-services/_static/image24.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image23.png)
7. Dopo aver immesso il nome utente e fa clic sul **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** per l'account Facebook:

    [![](external-authentication-services/_static/image26.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Abilitazione dell'autenticazione di Google

Google è di gran lunga il più semplice i servizi di autenticazione esterno per attivare perché non richiede un account sviluppatore, e non richiede informazioni aggiuntive, ad esempio l'ID dell'applicazione o una chiave privata come gli altri servizi di autenticazione esterno richiedere.

Per abilitare l'autenticazione di Google per l'applicazione web, procedere come segue:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image28.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image27.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Rimuovere il &quot; // &quot; caratteri per rimuovere il commento dalla riga di codice evidenziata e quindi ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Google è stata definita come un servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image30.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image29.png)
5. Quando si sceglie la **Google** pulsante, il browser verrà reindirizzato alla pagina di accesso di Google:

    [![](external-authentication-services/_static/image32.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image31.png)
6. Dopo aver immesso le credenziali Google e fare clic su **Accedi**, Google verrà richiesto di verificare che l'applicazione web abbia le autorizzazioni per accedere all'account Google:

    [![](external-authentication-services/_static/image34.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image33.png)
7. Quando fa clic su **Accept**, il browser web verrà reindirizzato all'applicazione web, che verranno richieste per il **nome utente** che si desidera associare all'account Google:

    [![](external-authentication-services/_static/image36.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image35.png)
8. Dopo aver immesso il nome utente e fa clic sui **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** per l'account Google:

    [![](external-authentication-services/_static/image38.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Abilitazione dell'autenticazione di Microsoft

È possibile creare un account per sviluppatore richiede l'autenticazione di Microsoft e richiede un ID client e segreto client per il funzionamento. Per informazioni sulla creazione di un account per sviluppatore Microsoft e come ottenere l'ID client e segreto client, vedere [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Una volta ottenuto il codice del cliente e il segreto, usare la procedura seguente per abilitare l'autenticazione di Microsoft per l'applicazione web:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image40.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image39.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Rimuovere il &quot; // &quot; caratteri per rimuovere il commento le righe evidenziate del codice e quindi aggiungere l'ID client e il segreto client. Dopo aver aggiunto tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Microsoft è stata definita come un servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image42.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image41.png)
5. Quando si sceglie la **Microsoft** pulsante, il browser verrà reindirizzato alla pagina di accesso di Microsoft:

    [![](external-authentication-services/_static/image44.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image43.png)
6. Dopo aver immesso le credenziali Microsoft e fare clic su **Accedi**, verrà richiesto di verificare che l'applicazione web abbia le autorizzazioni per accedere all'account Microsoft:

    [![](external-authentication-services/_static/image46.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image45.png)
7. Quando fa clic su **Yes**, il browser web verrà reindirizzato all'applicazione web, che verranno richieste per il **nome utente** che si desidera associare all'account Microsoft:

    [![](external-authentication-services/_static/image48.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image47.png)
8. Dopo aver immesso il nome utente e fa clic sui **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** del tuo account Microsoft:

    [![](external-authentication-services/_static/image50.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Abilitazione dell'autenticazione di Twitter

L'autenticazione è necessario creare un account sviluppatore, e richiede una chiave utente e il segreto per il funzionamento di Twitter. Per informazioni sulla creazione di un account sviluppatore Twitter e ottenere la chiave utente e il segreto del cliente, vedere [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Una volta ottenuto il codice del cliente e il segreto, usare la procedura seguente per abilitare l'autenticazione di Twitter per l'applicazione web:

1. Quando il progetto è aperto in Visual Studio 2013, aprire il *Startup.Auth.cs* file:

    [![](external-authentication-services/_static/image52.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image51.png)
2. Individuare la sezione evidenziata del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Rimuovere il &quot; // &quot; caratteri da rimuovere il commento le righe evidenziate del codice e quindi aggiungere il codice del cliente e il segreto. Dopo aver aggiunto tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Quando si preme F5 per aprire l'applicazione web nel browser web, si noterà che Twitter è stata definita come un servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image54.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image53.png)
5. Quando si fa clic il **Twitter** pulsante, il browser verrà reindirizzato alla pagina di accesso di Twitter:

    [![](external-authentication-services/_static/image56.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image55.png)
6. Dopo aver immesso le credenziali di Twitter e fare clic su **Authorize app**, il browser web verrà reindirizzato all'applicazione web, che verranno richieste per il **nome utente** che si desidera associare a l'account Twitter:

    [![](external-authentication-services/_static/image58.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image57.png)
7. Dopo aver immesso il nome utente e fa clic sui **iscriversi** pulsante, l'applicazione web verrà visualizzato il valore predefinito **home page di** dell'account Twitter:

    [![](external-authentication-services/_static/image60.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Informazioni aggiuntive

Per altre informazioni sulla creazione di applicazioni che usano OAuth e OpenID, vedere i seguenti URL:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>La combinazione di servizi di autenticazione esterni

Per una maggiore flessibilità, è possibile definire più servizi di autenticazione esterni allo stesso tempo - web gli utenti dell'applicazione possono usare un account da uno qualsiasi dei servizi di autenticazione esterni abilitate:

[![](external-authentication-services/_static/image62.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Configurazione di IIS Express per utilizzare un nome di dominio completo

Alcuni provider di autenticazione esterno non supportano il test dell'applicazione usando un indirizzo HTTP, ad esempio `http://localhost:port/`. Per risolvere questo problema, è possibile aggiungere un mapping statico completamente dominio nome completo (FQDN) per il file host e configurare le opzioni di progetto in Visual Studio 2013, usare il FQDN per il test e di debug. A tale scopo, procedere come segue:

- Aggiungere un nome di dominio completo statico mapping nel file HOSTS:

  1. Aprire un prompt dei comandi con privilegi elevati in Windows.
  2. Digitare il comando seguente:

      <kbd>il blocco note %WinDir%\system32\drivers\etc\hosts</kbd>
  3. Aggiungere una voce simile alla seguente al file HOSTS:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Salvare e chiudere il file HOSTS.

- Configurare il progetto di Visual Studio per usare il FQDN:

  1. Quando il progetto sia aperto in Visual Studio 2013, fare clic sui **progetto** menu, quindi selezionare le proprietà del progetto. Ad esempio, è possibile selezionare **WebApplication1 proprietà**.
  2. Selezionare il **Web** scheda.
  3. Immettere il nome FQDN per il <strong>Url del progetto</strong>. Ad esempio, immettere <kbd> <http://www.wingtiptoys.com> </kbd> se che era il mapping del nome di dominio completo che è stato aggiunto al file HOSTS.

- Configurare IIS Express per usare il FQDN per l'applicazione:

    1. Aprire un prompt dei comandi con privilegi elevati in Windows.
    2. Digitare il comando seguente per passare alla cartella IIS Express:

        <kbd>CD /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Digitare il comando seguente per aggiungere il nome di dominio completo per l'applicazione:

        <kbd>Appcmd.exe set config-section:system.applicationHost/sites / +&quot;[nome = 'WebApplication1'] .bindings. [ protocollo = 'http' bindingInformation ='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  In cui **WebApplication1** è il nome del progetto e **bindingInformation** contiene il numero di porta e il nome di dominio completo che si desidera usare per il test.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Come ottenere le impostazioni dell'applicazione per l'autenticazione di Microsoft

Il collegamento di un'applicazione a Windows Live per Microsoft Authentication è un processo semplice. Se un'applicazione a Windows Live non è stato già collegato, è possibile usare la procedura seguente:

1. Passare a [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) e immettere il nome dell'account Microsoft e la password quando richiesto, quindi fare clic su **Accedi**:

    [![](external-authentication-services/_static/image64.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image63.png)
2. Immettere il nome e la lingua dell'applicazione quando viene richiesto e quindi fare clic su **accetto**:

    [![](external-authentication-services/_static/image66.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image65.png)
3. Nel **le impostazioni API** pagina per l'applicazione, immettere il dominio di reindirizzamento per l'applicazione e copia le **ID Client** e **segreto Client** per il progetto e quindi Fare clic su **salvare**:

    [![](external-authentication-services/_static/image68.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Facoltativo: Disabilitare la registrazione locale

Le attuali funzionalità di registrazione locale ASP.NET non impediscano la creazione di account, membro di programmi automatizzati (Bot) ad esempio, usando una tecnologia bot-prevenzione e convalida come [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Per questo motivo, è necessario rimuovere il collegamento di form e la registrazione di account di accesso locale nella pagina di accesso. A tale scopo, aprire il  *\_cshtml* pagina nel progetto e impostare come commento le righe per il pannello di accesso locale e il collegamento di iscrizione. La pagina risultante dovrebbe essere analogo all'esempio di codice seguente:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Dopo che sono stati disabilitati il pannello di accesso locale e il collegamento di iscrizione, la pagina di accesso verrà visualizzati solo i provider di autenticazione esterno che sono stati abilitati:

[![](external-authentication-services/_static/image70.png "Fare clic per espandere l'immagine")](external-authentication-services/_static/image69.png)
