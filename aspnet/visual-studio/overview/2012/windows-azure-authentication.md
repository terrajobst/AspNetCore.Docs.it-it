---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure Authentication | Documenti Microsoft
author: Rick-Anderson
description: Gli strumenti Microsoft ASP.NET per Windows Azure Active Directory rende più semplice per abilitare l'autenticazione per applicazioni web ospitate in siti Web di Microsoft Azure...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 09cb37ceb0132958a48f5f3a5d52dc46c6f0a78d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="windows-azure-authentication"></a>Windows Azure Authentication
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Strumenti di Microsoft ASP.NET per Windows Azure Active Directory rende più semplice per abilitare l'autenticazione per applicazioni web ospitate in [siti Web di Azure](https://www.windowsazure.com/home/features/web-sites/). È possibile utilizzare l'autenticazione di Windows Azure per autenticare gli utenti di Office 365 dell'organizzazione, gli account aziendali sincronizzati da Active Directory locale o degli utenti creati in un dominio di Windows Azure Active Directory personalizzato. Abilitazione dell'autenticazione di Windows Azure consente di configurare l'applicazione per autenticare gli utenti con un solo [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenant.
> 
> Lo strumento autenticazione di Windows Azure ASP.NET non è supportato per i ruoli web in un servizio cloud, ma si prevede di eseguire questa operazione in una versione futura. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) è supportato in ruoli web di Windows Azure.
> 
> Per informazioni dettagliate su come configurare la sincronizzazione tra Active Directory locale e il tenant di Windows Azure Active Directory, vedere [utilizzo di ADFS 2.0 per implementare e gestire single sign-on](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory è attualmente disponibile come una [anteprima servizio gratuito](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Requisiti:

- Visual Studio 2012 o [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Estensioni per Visual Studio 2012 degli strumenti Web](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) o [estensioni degli strumenti Web per Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Strumenti di Microsoft ASP.NET per Windows Azure Active Directory Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) o [degli strumenti Microsoft ASP.NET per Windows Azure Active Directory: Visual Studio Express 2012 per Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Creare un'applicazione Web ASP.NET con Visual Studio 2012

È possibile creare qualsiasi applicazione Web con Visual Studio 2012, questa esercitazione viene utilizzato il modello intranet ASP.NET MVC.

1. Creare una nuova applicazione Intranet ASP.NET MVC 4 e accettare tutte le impostazioni predefinite. (Deve essere un **attraversamento** net e non In **ttere** net progetto).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Abilitare l'autenticazione di Windows Azure (quando si è un amministratore globale del principio)

Se non si dispone di un tenant esistente di Windows Azure Active Directory (ad esempio, tramite un account esistente di Office 365), è possibile creare un nuovo tenant di iscrizione per un [nuovo account di Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. Dal menu progetto selezionare **abilitare l'autenticazione di Windows Azure**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Immettere il dominio per il tenant di Windows Azure Active Directory (ad esempio, contoso.onmicrosoft.com) e fare clic su **abilitare**:

![](windows-azure-authentication/_static/image3.png)

3. Nell'autenticazione Web finestra di dialogo di accesso come amministratore per il tenant di Windows Azure Active Directory:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Abilitare Windows Azure da un amministratore non del principio

Se non si dispone dei privilegi di amministratore globale per il tenant di Windows Azure Active Directory, è possibile deselezionare la casella di controllo per il provisioning dell'applicazione.

![](windows-azure-authentication/_static/image6.png)

Verrà visualizzata la finestra di dialogo di **dominio**, **Id entità applicazione** e **URL di risposta** che sono necessari per il provisioning dell'applicazione con Azure Active Directory principio. È necessario fornire queste informazioni per un utente che dispone di privilegi sufficienti per eseguire il provisioning dell'applicazione. Vedere[sull'implementazione di single sign-on con Azure Active Directory di Windows - applicazione ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) per informazioni dettagliate su come usare i cmdlet per creare l'entità servizio manualmente.  
Dopo l'applicazione è stato correttamente eseguito il provisioning, è possibile fare clic su **continua ad aggiornare Web. config con le impostazioni selezionate**. Se si desidera continuare a sviluppare l'applicazione durante l'attesa per il provisioning viene eseguita, è possibile fare clic su **Chiudi per ricordare le impostazioni nel file di progetto**. La volta successiva che richiami abilitare l'autenticazione di Windows Azure, deselezionare la casella di controllo di provisioning, si noterà che le stesse impostazioni ed è possibile fare clic su **continua**, quindi fare clic su, **applicare queste impostazioni in Web. config**.

1. Attendere mentre l'applicazione è configurata per l'autenticazione di Windows Azure e il provisioning con Windows Azure Active Directory.
2. Dopo l'autenticazione di Windows Azure è stato abilitato per l'applicazione, fare clic su **Chiudi:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Premere F5 per eseguire l'applicazione. Si dovrebbe ottenere automaticamente reindirizzati alla pagina di accesso. Utilizzare le credenziali dell'utente principio directory eseguire l'accesso all'applicazione...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Poiché l'applicazione è attualmente in uso un certificato di prova autofirmato si riceverà un avviso dal browser che il certificato non è stato emesso da un'autorità di certificazione.

    Questo avviso può essere tranquillamente ignorato durante lo sviluppo locale, fare clic su **continua con il sito Web:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Sono ora correttamente eseguito l'accesso all'applicazione mediante l'autenticazione di Windows Azure.

    ![](windows-azure-authentication/_static/image2.jpg)

Attivazione di Windows Azure autenticazione effettua le seguenti modifiche all'applicazione:

- Un Anti-Cross-Site Request Forgery ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) classe ( *App\_Start\AntiXsrfConfig.cs* ) viene aggiunto al progetto.
- I pacchetti NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` viene aggiunto al progetto.
- Impostazioni di Windows Identity Foundation nell'applicazione verranno configurate per accettare i token di sicurezza dal tenant di Windows Azure Active Directory. Fare clic sull'immagine riportata di seguito per visualizzare una visualizzazione espansa delle modifiche apportate al *Web. config* file.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Viene eseguito il provisioning di un'entità servizio per l'applicazione nel tenant di Windows Azure Active Directory.
- HTTPS è abilitato.

## <a name="deploy-the-application-to-windows-azure"></a>Distribuire l'applicazione in Windows Azure

Per istruzioni dettagliate, vedere [si distribuisce un'applicazione Web ASP.NET a un sito Web di Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Per pubblicare un'applicazione mediante l'autenticazione di Windows Azure per un sito Web di Azure:

1. Fare clic con il pulsante destro sull'applicazione e selezionare **pubblica:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Nella finestra di dialogo Pubblica sito Web scaricare e importare un profilo di pubblicazione per il sito Web di Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. Il **connessione** scheda Mostra il **URL di destinazione** (URL rivolto al pubblico per l'applicazione). Fare clic su **convalida connessione** per testare la connessione:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Se è stata pubblicata per questo sito Web di Azure in precedenza è opportuno verificare il **Rimuovi file aggiuntivi nella destinazione** impostazione per garantire l'applicazione è stato pubblicato correttamente. Si noti il **abilitare l'autenticazione di Windows Azure** casella di controllo è slected.  

    ![](windows-azure-authentication/_static/image10.png)
5. Facoltativo: Nella **anteprima** fare clic sulla scheda **avviare Anteprima** per visualizzare i file distribuiti.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Fare clic su **pubblicare.**

    Verrà richiesto di abilitare l'autenticazione di Windows Azure per l'host di destinazione. Fare clic su **abilitare** continuare:

    ![](windows-azure-authentication/_static/image11.png)
7. Immettere le credenziali di amministratore per il tenant di Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Dopo aver pubblicato correttamente l'applicazione, un browser verrà aperto il sito web pubblicato.

    > [!NOTE]
    > Può richiedere fino a cinque minuti (in genere necessario meno) per l'applicazione di cui effettuare il provisioning con Windows Azure Active Directory completamente dopo avere abilitato l'autenticazione di Windows Azure per l'host di destinazione. Alla prima esecuzione dell'applicazione se viene visualizzato l'errore ACS50001: Relying party con nome '[area]' non è stato trovato, attendere qualche minuto, quindi provare a eseguire nuovamente l'applicazione.
9. Quando richiesto, accedere come un utente nella directory:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Dispone ora eseguito l'accesso di Azure ospitata l'applicazione utilizza l'autenticazione di Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problemi noti

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Autorizzazione basata sui ruoli si verifica un errore quando si utilizza l'autenticazione di Windows Azure < o: p >< / o: p >

L'autenticazione di Windows Azure non fornisce attualmente l'attestazione del ruolo necessarie in modo che l'autorizzazione basata sui ruoli può essere eseguite. Il ruolo dell'utente autenticato deve essere recuperato manualmente da Windows Azure Active Directory. < o: p >< / o: p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>La selezione di un'applicazione con l'autenticazione di Windows Azure genera l'errore "il dominio dell'utente connesso (live.com) non corrisponde a qualsiasi ACS20016 consentito dominio di questo servizio token di sicurezza" < o: p >< / o: p >

Se si è già connessi a un Account Microsoft (ad esempio hotmail.com, live.com, outlook.com) e si tenta di accedere a un'applicazione che ha abilitato l'autenticazione di Windows Azure che si riceva una risposta di 400 errore perché il dominio del tuo Account Microsoft non è riconosciuto da Windows Azure Active Directory. Per accedere all'applicazione, effettuare la disconnessione dal tuo Account Microsoft prima. < o: p >< / o: p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Registrazione in un'applicazione con attivata l'autenticazione di Windows Azure e un X509CertificateValidationMode diverso da None genera errori di convalida del certificato per il certificato AccessControl < o: p >< / o: p >

Convalida del certificato non è obbligatorio e deve essere lasciata disabilitato. L'identificazione personale del certificato dell'autorità di certificazione viene convalidato da WSFederationAuthenticationModule. < o: p >< / o: p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Quando si tenta di abilitare l'autenticazione di Windows Azure la finestra di dialogo di autenticazione Web viene visualizzato l'errore "ACS20016: il dominio dell'utente connesso (contoso.onmicrosoft.com) non corrisponde alcun dominio autorizzato di questo servizio token di sicurezza." < o: p >< / o: p >

Potrebbe essere visualizzato questo errore quando in precedenza correttamente registrato con un account Windows Azure Active Directory diverso dal processo di Visual Studio stesso. Disconnettersi dall'account specificato o riavviare Visual Studio. Se in precedenza effettuato l'accesso sia selezionata l'opzione "Mantieni l'accesso", potrebbe essere necessario cancellare i cookie del browser. < o: p >< / o: p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: La richiesta non è un messaggio del protocollo WS-Federation valido < o: p >< / o: p >

Questa situazione può verificarsi se è già stato effettuato con alcuni altri ID di Microsoft per uno dei servizi Azure. Finestra del browser uso privato, come in Internet Explorer InPrivate o Incognito in Chrome o cancellare tutti i cookie. <o:p></o:p>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Strumenti di Microsoft ASP.NET per Windows Azure Active Directory Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows le funzionalità di Azure: identità](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: sviluppare applicazioni per l'organizzazione](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: sviluppare App per più organizzazioni](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Come implementare single sign-on con Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Single Sign-On con Windows Azure Active Directory: un approfondimento](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Utilizzo di ADFS 2.0 per implementare e gestire single sign-on](https://technet.microsoft.com/library/jj205462.aspx)
