---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Aggiunta di form di ASP.NET Identity a un Web esistente o vuoto progetto | Documenti Microsoft
author: raquelsa
description: "In questa esercitazione viene illustrato come aggiungere identità di ASP.NET (il nuovo sistema di appartenenze di ASP.NET) a un'applicazione ASP.NET. Quando si crea un nuovo Web Form o MVC..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: f5783287a26174ddf65bb0eae34c347831d02c47
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Aggiunta di ASP.NET Identity a un Web esistente o vuoto form progetto
====================
da [Raquel Soares De Almeida](https://github.com/raquelsa)

> In questa esercitazione viene illustrato come aggiungere [ASP.NET Identity](introduction-to-aspnet-identity.md) (il nuovo sistema di appartenenze di ASP.NET) a un'applicazione ASP.NET.
> 
> Quando si crea un nuovo progetto di Web Form o MVC in Visual Studio 2013 RTM con account individuali, Visual Studio verrà installare tutti i pacchetti necessari e aggiungere tutte le classi necessarie per l'utente. In questa esercitazione verrà illustrati i passaggi per aggiungere il supporto di ASP.NET Identity per il progetto di Web Form esistente o un nuovo progetto vuoto. Verranno descritti tutti i pacchetti di NuGet che è necessario installare e le classi, che è necessario aggiungere. Esaminerà Web Form di esempio per la registrazione di nuovi utenti e la registrazione durante l'evidenziazione di tutte le API di punto di ingresso principale per l'autenticazione e gestione degli utenti. In questo esempio utilizzerà l'implementazione predefinita di ASP.NET Identity per l'archiviazione di dati SQL che si basa su Entity Framework. Questa esercitazione si utilizzerà del database locale per il database SQL.
> 
> In questa esercitazione è stato scritto dal Raquel Soares De Almeida e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Introduzione a ASP.NET Identity

1. Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Fare clic su **nuovo progetto** dall'inizio pagina oppure è possibile utilizzare il menu e selezionare **File**e quindi **nuovo progetto**.
3. Selezionare **Visual c# i** n riquadro a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Denominare il progetto "WebFormsIdentity" e quindi fare clic su **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
 Si noti il **Modifica autenticazione** pulsante è disabilitato e non viene fornito alcun supporto per l'autenticazione in questo modello. I modelli Web Form, MVC e Web API consentono di selezionare l'approccio di autenticazione. Per ulteriori informazioni, vedere [panoramica dell'autenticazione](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Aggiunta di pacchetti di identità applicazione in uso.

In Esplora risorse, mouse sul progetto e selezionare **Gestisci pacchetti NuGet**. Nella finestra di dialogo di testo di ricerca, digitare "*Identity.E*". Fare clic su Installa per questo pacchetto.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Si noti che questo pacchetto installerà i pacchetti di dipendenza: EntityFramework e Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Aggiunta di Web Form per registrare gli utenti

1. In **Esplora**, mouse sul progetto e fare clic su **Aggiungi**e quindi **Web Form**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. Nel **Specifica nome per l'elemento** nella finestra di dialogo Nome nuovo web form **registrare**, quindi fare clic su **OK**
3. Sostituire il markup generato *Register* file con il codice seguente. Le modifiche al codice sono evidenziati.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Questo è solo una versione semplificata di *Register* file creato quando si crea un nuovo progetto di Web Form ASP.NET. Il codice precedente aggiunge i campi del form e un pulsante per registrare un nuovo utente.
4. Aprire il *Register.aspx.cs* file e sostituire il contenuto del file con il codice seguente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Il codice precedente è una versione semplificata di *Register.aspx.cs* file creato quando si crea un nuovo progetto di Web Form ASP.NET.
    > 2. Il *IdentityUser* classe è l'implementazione di EntityFramework predefinita del *IUser* interfaccia. *IUser* è l'interfaccia minima per un utente in ASP.NET Identity Core.
    > 3. Il *nello UserStore* classe è l'implementazione di EntityFramework predefinita di un archivio utente. Questa classe implementa interfacce del ASP.NET Identity Core minimo: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* e *IUserRoleStore* .
    > 4. Il *UserManager* API che salvano automaticamente le modifiche a correlate all'utente di classe espone il *nello UserStore*.
    > 5. Il *IdentityResult* classe rappresenta il risultato di un'operazione di identità.
5. In **Esplora**, mouse sul progetto e fare clic su **Aggiungi**, **Aggiungi cartella ASP.NET** e quindi **App\_dati**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Aprire il *Web. config* file e aggiungere una stringa di connessione per il database verrà utilizzato per archiviare informazioni utente. Il database verrà creato in fase di esecuzione da EntityFramework per le entità di identità. La stringa di connessione è simile a quello creato automaticamente quando si crea un nuovo progetto di Web Form. Il codice evidenziato di seguito viene illustrato il markup che è necessario aggiungere:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Per Visual Studio 2015 o versione successiva, sostituire `(localdb)\v11.0` con `(localdb)\MSSQLLocalDB` nella stringa di connessione.
    
7. Fare clic con il pulsante destro file *Register* nel progetto e selezionare **imposta come pagina iniziale**. Premere Ctrl + F5 per compilare ed eseguire l'applicazione web. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity dispone del supporto per la convalida e in questo esempio è possibile verificare il comportamento predefinito e la Password utente validator provenienti dal pacchetto di base di identità. Il validator predefinito per l'utente (`UserValidator`) ha una proprietà `AllowOnlyAlphanumericUserNames` che ha il valore predefinito impostato su `true`. Il validator predefinito per la Password (`MinimumLengthValidator`) garantisce che la password è di almeno 6 caratteri. Queste convalide presenti proprietà `UserManager` che può essere sottoposto a override se si desidera che la convalida personalizzata,

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Verifica per determinare se il Database di identità del database locale e tabelle generate da Entity Framework

1. Nel **vista** menu, fare clic su **Esplora Server**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Espandere **DefaultConnection (WebFormsIdentity)**, espandere **tabelle**, fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Configurazione dell'applicazione per l'autenticazione OWIN

A questo punto è stato aggiunto solo il supporto per la creazione di utenti. A questo punto, verrà per illustrare come è possibile aggiungere l'autenticazione per accedere a un utente. Identità di ASP.NET utilizza middleware di autenticazione di Microsoft OWIN per l'autenticazione basata su form. L'autenticazione dei Cookie OWIN un cookie e attestazioni meccanismo di autenticazione in base che può essere utilizzato da qualsiasi framework ospitati in [OWIN](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx) o IIS. Con questo modello, è possono utilizzare gli stessi pacchetti di autenticazione in più Framework, ad esempio ASP.NET MVC e Web Form. Per ulteriori informazioni sul progetto Katana e come eseguire middleware in un host indipendente vedere [introduzione con il progetto Katana](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>L'installazione di pacchetti di autenticazione all'applicazione

1. In Esplora risorse, mouse sul progetto e selezionare **Gestisci pacchetti NuGet**. Nella finestra di dialogo di testo di ricerca, digitare "*Identity.Owin*". Fare clic su Installa per questo pacchetto.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Cerca pacchetto ***systemweb*** e installarlo.   

    > [!NOTE]
    > Il **italiano** pacchetto contiene un set di classi di estensione OWIN per gestire e configurare il middleware di autenticazione OWIN devono essere utilizzate dai pacchetti di ASP.NET Identity Core.  
    > Il **systemweb** pacchetto contiene un server OWIN che consente alle applicazioni basate su OWIN per l'esecuzione in IIS tramite la pipeline della richiesta ASP.NET. Per ulteriori informazioni vedere [Middleware OWIN in IIS è integrata pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Aggiunta di classi di configurazione di autenticazione e di avvio di OWIN

1. In **Esplora**mouse sul progetto, fare clic su **Aggiungi**e quindi **Aggiungi nuovo elemento**. Nella finestra di dialogo di testo di ricerca, digitare "*owin*". Nome della classe "*avvio*" e fare clic su **Aggiungi**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Nel file Startup.cs, aggiungere il codice evidenziato mostrato di seguito per configurare l'autenticazione dei cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Questa classe contiene il `OwinStartup` attributo per specificare la classe di avvio OWIN. Ogni applicazione OWIN dispone di una classe di avvio in cui è necessario specificare i componenti per la pipeline dell'applicazione. Vedere [OWIN avvio classe rilevamento](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) per ulteriori informazioni su questo modello.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Aggiunta di Web Form per la registrazione e la connessione degli utenti

1. Aprire il *Register.cs* file e aggiungere il codice seguente che avranno accesso all'utente quando la registrazione ha esito positivo. Le modifiche sono evidenziate.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Poiché ASP.NET Identity e autenticazione dei Cookie OWIN sistema basata sulle attestazioni, il framework richiede allo sviluppatore di app generare un [ClaimsIdentity](https://msdn.microsoft.com/en-us/library/microsoft.identitymodel.claims.claimsidentity.aspx) per l'utente. ClaimsIdentity dispone di informazioni su tutte le attestazioni per l'utente, ad esempio i ruoli a cui appartiene l'utente. È anche possibile aggiungere ulteriori attestazioni per l'utente in questa fase.
    > - Può accedere l'utente usando AuthenticationManager da OWIN e chiamare il metodo `SignIn` e passando l'oggetto ClaimsIdentity come illustrato in precedenza. Questo codice verrà accesso dell'utente e generare anche un cookie. Questa chiamata è analoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizzato il [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) modulo.
2. In **Esplora**, fare clic il progetto **Aggiungi**e quindi **Web Form**. Nome del web form **accesso**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Sostituire il contenuto del *Login.aspx* file con il codice seguente:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Sostituire il contenuto del *Login.aspx.cs* file con le operazioni seguenti:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Il `Page_Load` ora controlla lo stato dell'utente corrente e accetta azione in base alle relative `Context.User.Identity.IsAuthenticated` stato.  
    >     **Visualizzare connesso in nome utente** : Microsoft ASP.NET Identity Framework ha aggiunto i metodi di estensione in [IIdentity](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.aspx) che consente di ottenere il `UserName` e `UserId` per il Utente connesso. Questi metodi di estensione sono definiti nel `Microsoft.AspNet.Identity.Core` assembly. Questi metodi di estensione sono la sostituzione di [HttpContext.User.Identity.Name](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.user.aspx) .
    > - Metodo di accesso:   
    >     `This`metodo sostituisce la precedente `CreateUser_Click` metodo in questo esempio e l'ora consente l'accesso l'utente dopo la creazione dell'utente.   
    >  Il Framework di Microsoft OWIN ha aggiunto i metodi di estensione in `System.Web.HttpContext` che consente di ottenere un riferimento a un `IOwinContext`. Questi metodi di estensione sono definiti `Microsoft.Owin.Host.SystemWeb` assembly. Il `OwinContext` classe espone un `IAuthenticationManager` proprietà che rappresenta la funzionalità middleware di autenticazione disponibile sulla richiesta corrente.  
    >  Può accedere l'utente usando il `AuthenticationManager` da OWIN e chiamare il metodo `SignIn` e passando il `ClaimsIdentity` come illustrato in precedenza.   
    >  Poiché ASP.NET Identity e autenticazione dei Cookie OWIN sono sistema basata sulle attestazioni, il framework richiede che l'app per generare un `ClaimsIdentity` per l'utente.   
    >  Il `ClaimsIdentity` dispone di informazioni su tutte le attestazioni per l'utente, ad esempio i ruoli a cui appartiene l'utente. È inoltre possibile aggiungere ulteriori attestazioni per l'utente in questa fase  
    >  Questo codice verrà accesso dell'utente e generare anche un cookie. Questa chiamata è analoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizzato il [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) modulo.
    > - `SignOut`metodo:   
    >  Ottiene un riferimento di `AuthenticationManager` da OWIN e chiama `SignOut`. Questo comportamento è analogo a [FormsAuthentication. SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx) utilizzato dal metodo di [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) modulo.
5. Premere **Ctrl + F5** per compilare ed eseguire l'applicazione web. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
 Nota: A questo punto, il nuovo utente viene creato e registrato.
6. Fare clic su **disconnettersi** pulsante. Si verrà reindirizzati per il Log nel modulo.
7. Immettere un nome utente non valido o password e fare clic **Accedi** pulsante.   
 Il `UserManager.Find` metodo restituirà null e il messaggio di errore: " *nome utente non valido o password* " verranno visualizzati.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
