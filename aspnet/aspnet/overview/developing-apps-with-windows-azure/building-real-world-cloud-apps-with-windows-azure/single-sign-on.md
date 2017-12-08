---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Single Sign-On (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: f0d465b363652c691c203d608f2cb9d139e72fed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Single Sign-On (creazione di applicazioni Cloud reale in Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Vi sono molti problemi di sicurezza da considerare quando si sta sviluppando un'applicazione cloud, ma per questa serie l'attenzione verrà posta su un solo: l'accesso single sign-on. Persone una domanda spesso chiedono è: "sono principalmente la creazione di App per i dipendenti della società; come ospitare le app nel cloud e attivarli utilizzare lo stesso modello di sicurezza che dipendenti conoscono e utilizzano nell'ambiente locale quando si eseguono applicazioni che sono ospitati all'interno del firewall?" Uno dei modi che è abilitare questo scenario viene chiamato Azure Active Directory (Azure AD). Azure AD consente di rendere enterprise line-of-business (LOB) App disponibili su Internet e consente di rendere disponibili queste App ai partner commerciali nonché.

## <a name="introduction-to-azure-ad"></a>Introduzione ad Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) fornisce [Active Directory](https://msdn.microsoft.com/en-us/library/windows/desktop/aa746492.aspx) nel cloud. Di seguito sono elencate le funzionalità principali:

- Si integra con Active Directory locale.
- Consente di single sign-on con App.
- Supporta gli standard aperti, ad esempio [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), e [OAuth 2.0](http://oauth.net/2/).
- Enterprise supporta [API REST Graph](https://msdn.microsoft.com/en-us/library/hh974476.aspx).

Si supponga di che avere un ambiente di Windows Server Active Directory locale utilizzato per consentire ai dipendenti di accedere alle applicazioni Intranet:

![](single-sign-on/_static/image1.png)

Azure AD consente di eseguire la creazione di una directory nel cloud. È una funzionalità disponibile gratuitamente e semplice da configurare.

Può essere completamente indipendente da Active Directory locale; è possibile inserire tutti gli utenti si desidera in essa contenuti e l'autenticazione in applicazioni Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Oppure è possibile integrarlo con locale Active Directory.

![Active Directory e l'integrazione di WAAD](single-sign-on/_static/image3.png)

Ora tutti i dipendenti che possono eseguire l'autenticazione locale possono anche eseguire l'autenticazione tramite Internet senza dover aprire un firewall o distribuire tutti i nuovi server nel data center. È possibile continuare a sfruttare tutti l'ambiente Active Directory esistente che si conosce e utilizzare oggi per fornire l'App interne single sign-on funzionalità.

Dopo aver apportato questa connessione tra Active Directory e Azure AD, è inoltre possibile abilitare le app web e i dispositivi mobili per autenticare i dipendenti nel cloud ed è possibile abilitare le app di terze parti, ad esempio Office 365, SalesForce.com o Google apps, per accettare il credenziali dei dipendenti. Se si usa Office 365, sta già impostare con Azure AD Poiché Office 365 Usa Azure AD per l'autenticazione e autorizzazione.

![app di terze parti 3rd](single-sign-on/_static/image4.png)

Il vantaggio di questo approccio è che ogni volta che l'organizzazione aggiunge o elimina un utente, o un utente modifica una password, utilizzare lo stesso processo utilizzato oggi nel proprio ambiente locale. Tutti di locale vengono automaticamente propagate le modifiche di Active Directory nell'ambiente cloud.

Se la società utilizza o lo spostamento in Office 365, buone notizie consiste nel fatto che è necessario impostare automaticamente poiché Office 365 Usa Azure AD per l'autenticazione di Azure AD. Pertanto nelle proprie applicazioni è possibile utilizzare facilmente lo stesso di autenticazione che usa Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurare un tenant di Azure AD

una directory di Azure AD viene chiamata un Azure AD [tenant](https://technet.microsoft.com/en-us/library/jj573650.aspx), e la configurazione di un tenant è piuttosto semplice. Vi mostreremo come viene eseguita nel portale di gestione di Azure per illustrare i concetti, ma ovviamente come le altre funzioni del portale è possibile anche farlo usando uno script o l'API di gestione.

Nel portale di gestione fare clic sulla scheda Active Directory.

![Nel portale di WAAD](single-sign-on/_static/image5.png)

È automaticamente un tenant di Azure AD per l'account di Azure che è possibile scegliere di **Aggiungi** nella parte inferiore della pagina per creare directory aggiuntive. È necessario uno per un ambiente di test e uno per la produzione, ad esempio. Considerare con attenzione che cos' denominare una nuova directory. Se si utilizza il nome della directory e quindi utilizzare il nome del nuovo ad uno degli utenti, che può generare confusione.

![Aggiungi directory](single-sign-on/_static/image6.png)

Il portale fornisce supporto completo per la creazione, eliminazione e gestione degli utenti all'interno di questo ambiente. Ad esempio, per aggiungere un utente, andare al **utenti** scheda e fare clic il **Aggiungi utente** pulsante.

![Pulsante Aggiungi utente](single-sign-on/_static/image7.png)

![Finestra Aggiungi utente](single-sign-on/_static/image8.png)

È possibile creare un nuovo utente che esiste solo in questa directory oppure è possibile registrare un Account Microsoft come un utente in questa directory, o di registro o un utente da un'altra directory di Azure AD come utente in questa directory. (In una directory reale, il dominio predefinito sarebbe ContosoTest.onmicrosoft.com. È inoltre possibile utilizzare un dominio di propria scelta, ad esempio contoso.com.)

![Tipi di utente](single-sign-on/_static/image9.png)

![Finestra Aggiungi utente](single-sign-on/_static/image10.png)

È possibile assegnare l'utente a un ruolo.

![Profilo utente](single-sign-on/_static/image11.png)

E l'account viene creato con una password temporanea.

![Password temporanea](single-sign-on/_static/image12.png)

Create in questo modo gli utenti possono accedere immediatamente alle App web utilizzando la directory cloud.

Che cos'è ideale per enterprise single sign-on, tuttavia, è il **integrazione Directory** scheda:

![Nella scheda Integrazione directory](single-sign-on/_static/image13.png)

Se si abilita l'integrazione di directory, e [scaricare uno strumento](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), è possibile sincronizzare la directory cloud con l'ambiente locale Active Directory esistente che già in uso all'interno dell'organizzazione. Quindi tutti gli utenti archiviati nella directory verrà visualizzata in questa directory cloud. App cloud può ora eseguire l'autenticazione tutti i dipendenti utilizzando le credenziali di Active Directory esistente. E liberare tutto: sia lo strumento di sincronizzazione e di Azure Active Directory stessa.

Lo strumento è una procedura guidata che è facile da usare, come si può notare da queste schermate. Non sono istruzioni complete, solo un esempio che mostra il processo di base. Per ulteriori informazioni procedure-to--it, vedere i collegamenti nella [risorse](#resources) sezione alla fine del capitolo.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image14.png)

Fare clic su **Avanti**e quindi immettere le credenziali di Azure Active Directory.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image15.png)

Fare clic su **Avanti**, quindi immettere locale le credenziali di Active Directory.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image16.png)

Fare clic su **Avanti**e quindi indicare se si desidera archiviare un hash della password di Active Directory nel cloud.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image17.png)

L'hash della password che è possibile archiviare nel cloud è un hash unidirezionale; password non vengono mai archiviate in Azure AD. Se si decide di archiviazione degli hash nel cloud, sarà necessario utilizzare [Active Directory Federation Services](https://technet.microsoft.com/en-us/library/hh831502.aspx) (ADFS). Sono inoltre disponibili [ad altri fattori da considerare quando si sceglie di utilizzare ADFS o meno](https://technet.microsoft.com/en-us/library/jj573653.aspx). L'opzione di ADFS richiede alcuni passaggi di configurazione aggiuntivi.

Se si sceglie di archiviare l'hash nel cloud, è terminato e lo strumento si avvia la sincronizzazione delle directory quando si fa clic **Avanti**.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image18.png)

E in pochi minuti è terminato.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image19.png)

È necessario solo eseguire questo comando in un controller di dominio dell'organizzazione, in Windows 2003 o versione successiva. E non è necessario riavviare il computer. Quando è terminato, tutti gli utenti sono nel cloud ed è possibile eseguire single sign-on da qualsiasi web o applicazione per dispositivi mobili, tramite SAML, OAuth o WS-Fed.

In alcuni casi è spesso richiesto sulla modalità di protezione si tratta: utilizzato da Microsoft, per i propri dati aziendali sensibili? E la risposta è sì che facciamo. Ad esempio, se si passa al sito Microsoft SharePoint interno [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), viene richiesto di accedere.

![Accedi Office 365](single-sign-on/_static/image20.png)

Microsoft ha abilitato ADFS, pertanto quando si immette un ID Microsoft, l'utente viene reindirizzato a una pagina di accesso di ADFS.

![Accedi ad FS](single-sign-on/_static/image21.png)

E, dopo aver immesso le credenziali archiviate in un account Microsoft AD interno, si ha accesso a questa applicazione interna.

![Sito di SharePoint MS](single-sign-on/_static/image22.png)

Si usa un server di accesso AD principalmente perché è già stato ADFS impostare prima di Azure AD è diventato disponibile, ma il processo di log viene eseguita tramite una directory di Azure AD nel cloud. Si inseriscono i documenti importanti, controllo del codice sorgente, file di gestione delle prestazioni, report sulle vendite e altre informazioni, nel cloud e si utilizza questa soluzione stesso esatto per proteggerli.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Creare un'applicazione ASP.NET che usa Azure AD per single sign-on

Visual Studio rende molto semplice creare un'applicazione che utilizza Azure AD per single sign-on, come si può notare da qualche catture di schermata.

Quando si crea una nuova applicazione ASP.NET, MVC o Web Form, il metodo di autenticazione predefinito è l'identità di ASP.NET. Per modificare che ad Azure AD, si fa clic su un **Modifica autenticazione** pulsante.

![Modifica autenticazione](single-sign-on/_static/image23.png)

Selezionare un account aziendale, immettere il nome di dominio e quindi selezionare Single Sign-On.

![Configurare una finestra di dialogo autenticazione](single-sign-on/_static/image24.png)

È possibile inoltre consentire la lettura di app o accesso in lettura/scrittura per i dati di directory. Se a tale scopo, è possibile utilizzare il [API REST Graph di Azure](https://msdn.microsoft.com/en-us/library/windowsazure/hh974476.aspx) per cercare il numero di telefono degli utenti, verificare che siano in ufficio, sono ora dell'ultimo accesso, e così via.

Questo è tutto che è necessario effettuare - Visual Studio chiede le credenziali per un amministratore tenant di Azure AD, e quindi Configura il progetto sia il tenant di Azure AD per la nuova applicazione.

Quando si esegue il progetto, si noterà una pagina di accesso ed è possibile accedere con credenziali di un utente nella directory di Azure AD.

![Org account Accedi](single-sign-on/_static/image25.png)

![Connesso](single-sign-on/_static/image26.png)

Quando si distribuisce l'app in Azure, sarà sufficiente è selezionato un **Abilita autenticazione aziendale** casella di controllo e ancora una volta Visual Studio si occupa di tutta la configurazione per l'utente.

![Pubblicazione del sito Web](single-sign-on/_static/image27.png)

Questi screenshot provengono da un'esercitazione dettagliata completezza che illustra come compilare un'applicazione che utilizza l'autenticazione di Azure AD: [lo sviluppo di applicazioni ASP.NET con Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Riepilogo

In questo capitolo è emerso che Azure Active Directory, Visual Studio e ASP.NET, semplificano impostare single sign-on in applicazioni di Internet per gli utenti dell'organizzazione. Gli utenti possono accedere in applicazioni Internet con le stesse credenziali che vengono utilizzare per l'accesso utilizzando Active Directory nella rete interna.

Il [capitolo successivo](data-storage-options.md) esamina le opzioni di archiviazione di dati disponibili per un'app cloud.

<a id="resources"></a>
## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse:

- [Documentazione di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Pagina del portale per la documentazione di Azure AD nel sito Web windowsazure.com. Per esercitazioni dettagliate, vedere il **sviluppare** sezione.
- [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Pagina del portale per la documentazione sull'autenticazione a più fattori in Azure.
- [Le opzioni di autenticazione account aziendale](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Spiegazione delle opzioni di autenticazione di Azure AD nella finestra di dialogo Nuovo progetto Visual Studio 2013.
- [Microsoft Patterns and Practices - federazione identità modello](https://msdn.microsoft.com/en-us/library/dn589790.aspx).
- [Procedura: Installare lo strumento di sincronizzazione di Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 mappa del contenuto](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Collegamenti alla documentazione su ad FS 2.0.
- [Autorizzazione basata sui ruoli e su ACL in un'applicazione di Microsoft Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Applicazione di esempio.
- [Blog di Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).
- [Controllo dell'accesso in BYOD e integrazione di Directory in un'infrastruttura di identità ibrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech 2014 Ed sessione video da Gayana Bagdasaryan.

>[!div class="step-by-step"]
[Precedente](web-development-best-practices.md)
[Successivo](data-storage-options.md)
