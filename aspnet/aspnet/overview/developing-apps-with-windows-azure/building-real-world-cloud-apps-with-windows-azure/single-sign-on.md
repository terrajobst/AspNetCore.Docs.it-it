---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: L'accesso Single Sign-On (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: feace28c373a767453b1b984f6fdc5a3d97a9217
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834317"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Single Sign-On (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Esistono molti problemi di protezione da considerare quando si sviluppa un'app cloud, ma per questa serie ci concentreremo solo su uno: l'accesso single sign-on. Persone una domanda spesso chiedono è questo: "Io stia principalmente creando App per i dipendenti della mia azienda; come ospitare le app nel cloud e comunque consentire loro di usare lo stesso modello di sicurezza dipendenti conosceranno e usano nell'ambiente locale quando si eseguono le app che sono ospitati all'interno del firewall?" Azure Active Directory (Azure AD) viene chiamato uno dei modi che è abilitare questo scenario. Azure AD consente di rendere enterprise line-of-business (LOB) disponibili le applicazioni su Internet e consente di rendere disponibili ai partner commerciali anche queste app.

## <a name="introduction-to-azure-ad"></a>Introduzione ad Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) fornisce [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) nel cloud. Le funzionalità chiave includono quanto segue:

- Si integra con Active Directory locale.
- Abilita single sign-on con le tue app.
- Supporta standard aperti, ad esempio [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), e [OAuth 2.0](http://oauth.net/2/).
- Enterprise supporta [API REST Graph](https://msdn.microsoft.com/library/hh974476.aspx).

Si supponga di che avere un ambiente di Windows Server Active Directory in locale che consente di abilitare i dipendenti accedano alle app di Intranet:

![](single-sign-on/_static/image1.png)

Azure AD consente di eseguire operazioni quali è possibile creare una directory nel cloud. È una funzionalità gratuita e semplice da configurare.

Può essere completamente indipendente da Active Directory locale; è possibile inserire tutti gli utenti desiderati in esso e autenticarli in applicazioni da Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Oppure è possibile integrarlo con on-premises AD.

![AD e l'integrazione di WAAD](single-sign-on/_static/image3.png)

Ora tutti i dipendenti possono eseguire l'autenticazione in locale possono anche eseguire l'autenticazione tramite Internet, senza dover aprire un firewall o distribuire nuovi server nel data center. È possibile continuare a sfruttare tutti l'ambiente Active Directory esistente che si conosce e utilizzare oggi stesso per fornire le applicazioni interne accesso single sign-on funzionalità.

Dopo aver apportato questa connessione tra Active Directory e Azure AD, è anche possibile abilitare le app web e i dispositivi mobili per autenticare i dipendenti nel cloud ed è possibile abilitare le app di terze parti, ad esempio Office 365, SalesForce.com o Google apps, per accettare le credenziali dei dipendenti. Se si usa Office 365, sta già configurare con Azure AD perché Office 365 Usa Azure AD per l'autenticazione e autorizzazione.

![app di terze parti 3rd](single-sign-on/_static/image4.png)

Il vantaggio di questo approccio è che ogni volta che l'organizzazione aggiunge o elimina un utente, o un utente modifica una password, è utilizzare lo stesso processo già usate in locale nell'ambiente in uso. Tutti di on-premises AD modifiche vengono automaticamente propagate nell'ambiente cloud.

Se l'azienda Usa o lo spostamento in Office 365, la buona notizia è che è possibile configurare automaticamente poiché Office 365 Usa Azure AD per l'autenticazione di Azure AD. Pertanto, è possibile facilmente usare nelle proprie applicazioni la stessa autenticazione che usa Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurare un tenant di Azure AD

Azure Active directory viene chiamata un Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), e la configurazione di un tenant è piuttosto semplice. Vi mostreremo come farlo nel portale di gestione di Azure per illustrare i concetti, ma naturalmente, come le altre funzioni del portale è possibile anche farlo usando uno script o API di gestione.

Nel portale di gestione fare clic sulla scheda Active Directory.

![WAAD nel portale](single-sign-on/_static/image5.png)

È automaticamente un tenant di Azure AD per l'account di Azure che è possibile fare clic il **Add** nella parte inferiore della pagina per creare directory aggiuntive. È possibile uno per un ambiente di test e uno per la produzione, ad esempio. Si pensi attentamente cosa si denominare una nuova directory. Se si usa il nome della directory e quindi si utilizza il nome del nuovo ad uno degli utenti, che può generare confusione.

![Aggiungi directory](single-sign-on/_static/image6.png)

Il portale offre supporto completo per la creazione, eliminazione e la gestione degli utenti all'interno di questo ambiente. Ad esempio, per aggiungere un utente, visitare il **gli utenti** scheda e fare clic sul **Aggiungi utente** pulsante.

![Pulsante Aggiungi utente](single-sign-on/_static/image7.png)

![Aggiungi finestra di dialogo utente](single-sign-on/_static/image8.png)

È possibile creare un nuovo utente che esiste solo in questa directory oppure è possibile registrare un Account Microsoft come un utente in questa directory, o registrazione o un utente da un'altra directory di Azure AD come utente in questa directory. (In una directory reale, il dominio predefinito sarebbe ContosoTest.onmicrosoft.com. È anche possibile usare un dominio di propria scelta, ad esempio contoso.com.)

![Tipi utente](single-sign-on/_static/image9.png)

![Aggiungi finestra di dialogo utente](single-sign-on/_static/image10.png)

È possibile assegnare l'utente a un ruolo.

![Profilo utente](single-sign-on/_static/image11.png)

E l'account viene creato con una password temporanea.

![Password temporanea](single-sign-on/_static/image12.png)

Create in questo modo gli utenti possono accedere immediatamente alle App web usando questa directory cloud.

Che cos'è ottimo per enterprise single sign-on, tuttavia, è il **integrazione di Directory** scheda:

![Nella scheda Integrazione directory](single-sign-on/_static/image13.png)

Se si abilita l'integrazione di directory, e [scaricare uno strumento](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), è possibile sincronizzare questa directory cloud con esistente locale Active Directory che è già in uso all'interno dell'organizzazione. Quindi tutti gli utenti archiviati nella directory verrà visualizzata in questa directory cloud. Le app cloud possono ora eseguire l'autenticazione tutti i dipendenti con le credenziali di Active Directory esistente. E tutto ciò è gratuito: sia lo strumento di sincronizzazione e Azure AD.

Lo strumento è una procedura guidata che è facile da usare, come si può notare da queste schermate. Non sono istruzioni complete, solo un esempio che illustra il processo di base. Per informazioni dettagliate how-to--it, vedere i collegamenti nella [risorse](#resources) sezione alla fine del capitolo.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image14.png)

Fare clic su **successivo**e quindi immettere le credenziali di Azure Active Directory.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image15.png)

Fare clic su **successivo**e quindi immettere in locale le credenziali di Active Directory.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image16.png)

Fare clic su **successivo**e quindi indicare se si desidera archiviare un hash della password Active Directory nel cloud.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image17.png)

L'hash della password che è possibile archiviare nel cloud è un hash unidirezionale; le password effettive non vengono mai archiviate in Azure AD. Se si decide di archiviare hash nel cloud, è possibile usare [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ad FS). Sono inoltre disponibili [altri fattori da considerare quando si sceglie di usare ad FS o meno](https://technet.microsoft.com/library/jj573653.aspx). L'opzione di ad FS richiede alcuni ulteriori passaggi di configurazione.

Se si sceglie di archiviare hash nel cloud e aver completato lo strumento si avvia la sincronizzazione delle directory quando si fa clic **successivo**.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image18.png)

E in pochi minuti termine.

![Configurazione guidata dello strumento di sincronizzazione di WAAD](single-sign-on/_static/image19.png)

Sufficiente eseguire questo strumento in un controller di dominio nell'organizzazione, in Windows 2003 o versione successiva. E non è necessario riavviare. Quando è terminato, tutti gli utenti sono nel cloud ed è possibile eseguire single sign-on da qualsiasi applicazione web o per dispositivi mobili, tramite SAML, OAuth e WS-Fed.

In alcuni casi domanda su come proteggere si tratta, vengono utilizzate da Microsoft, per i propri dati aziendali sensibili? E la risposta è affermativa che facciamo. Ad esempio, se si passa al sito di SharePoint di Microsoft interno al [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), viene richiesto di accedere.

![Accesso aggiuntivo per Office 365](single-sign-on/_static/image20.png)

Microsoft ha abilitato ad FS, in modo che quando si immette un ID Microsoft, ottiene reindirizzate a una pagina di accesso di ad FS.

![Accedi ad FS](single-sign-on/_static/image21.png)

E dopo aver immesso le credenziali archiviate in un account Microsoft AD interno, si ha accesso a questa applicazione interna.

![Sito di SharePoint MS](single-sign-on/_static/image22.png)

Si intende usare un server di accesso AD principalmente perché abbiamo già avuto ADFS configurato prima di Azure AD è diventato disponibile, ma il processo di log è in corso una directory di Azure AD nel cloud. È inserire i documenti importanti, controllo del codice sorgente, file di gestione delle prestazioni, analisi delle vendite e altro ancora, nel cloud e si usa questa soluzione stesso esatta per proteggerle.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Creare un'app ASP.NET che usa Azure AD per single sign-on

Visual Studio rende molto semplice creare un'app che usa Azure AD per single sign-on, come si può notare da alcune schermate.

Quando si crea una nuova applicazione ASP.NET, MVC o Web Form, il metodo di autenticazione predefinito è ASP.NET Identity. Per modificare che Azure ad, si fa clic su un **Modifica autenticazione** pulsante.

![Modifica autenticazione](single-sign-on/_static/image23.png)

Selezionare gli account dell'organizzazione, immettere il nome di dominio e quindi selezionare Single Sign-On.

![Finestra di dialogo di autenticazione Configura](single-sign-on/_static/image24.png)

È possibile anche assegnare le app di lettura o lettura/scrittura l'autorizzazione per i dati della directory. Se tale scopo, è possibile utilizzare il [API REST Graph di Azure](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) per cercare il numero di telefono degli utenti, scoprire se sono in ufficio, sono ora dell'ultimo accesso via e così via.

Questo è tutto che è necessario eseguire - Visual Studio chiede le credenziali di amministratore per il tenant di Azure AD e quindi Configura il progetto sia il tenant di Azure AD per la nuova applicazione.

Quando si esegue il progetto, si vedrà una pagina di accesso ed è possibile accedere con le credenziali di un utente nel tenant di Azure ad.

![L'organizzazione accesso all'account](single-sign-on/_static/image25.png)

![Eseguito l'accesso](single-sign-on/_static/image26.png)

Quando si distribuisce l'app in Azure, tutto ciò che devi fare è selezionare una **Abilita autenticazione aziendale** casella di controllo e ancora una volta Visual Studio si occupa di tutto la configurazione per l'utente.

![Pubblica sito Web](single-sign-on/_static/image27.png)

Questi screenshot provengono da un'esercitazione dettagliata completa che illustra come compilare un'app che usa l'autenticazione di Azure AD: [lo sviluppo di App ASP.NET con Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Riepilogo

In questo capitolo si è visto che Azure Active Directory, Visual Studio e ASP.NET, renderlo semplice da configurare single sign-on in applicazioni di Internet per gli utenti dell'organizzazione. Gli utenti possono accedere applicazioni da Internet usando le stesse credenziali che usano per accedere utilizzando Active Directory nella rete interna.

Il [capitolo successivo](data-storage-options.md) esamina le opzioni di archiviazione di dati disponibili per un'app cloud.

<a id="resources"></a>
## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse:

- [Documentazione di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Pagina del portale per la documentazione di Azure AD nel sito Web windowsazure.com. Per esercitazioni dettagliate, vedere la **sviluppare** sezione.
- [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Pagina del portale per la documentazione sull'autenticazione a più fattori in Azure.
- [Le opzioni di autenticazione di account dell'organizzazione](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Spiegazione delle opzioni di autenticazione Azure AD nella finestra di dialogo Nuovo progetto Visual Studio 2013.
- [Microsoft Patterns and Practices - modello di identità federativa](https://msdn.microsoft.com/library/dn589790.aspx).
- [Procedura: Installare lo strumento Azure Active Directory Sync](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 mappa del contenuto](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Collegamenti alla documentazione su ad FS 2.0.
- [Autorizzazione basata sui ruoli e su ACL in un'applicazione di Microsoft Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Applicazione di esempio.
- [Blog di Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).
- [Controllo di accesso di BYOD e integrazione di Directory in un'infrastruttura di identità ibrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 sessione video da Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Precedente](web-development-best-practices.md)
> [Successivo](data-storage-options.md)
