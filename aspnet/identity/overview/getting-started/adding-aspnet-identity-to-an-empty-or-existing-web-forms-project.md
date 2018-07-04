---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Aggiunta di progetto di form di ASP.NET Identity a una Web vuoto o esistente | Microsoft Docs
author: raquelsa
description: Questa esercitazione illustra come aggiungere ASP.NET Identity (il nuovo sistema di appartenenze per ASP.NET) a un'applicazione ASP.NET. Quando si crea un nuovo Web Form o MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: eb2605043be49754b0641f0e919a5bd7c2f639b1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393280"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Progetto di form di ASP.NET Identity aggiunta a un Web vuoto o esistente
====================
da [Raquel Soares De Almeida](https://github.com/raquelsa)

> Questa esercitazione illustra come aggiungere [ASP.NET Identity](introduction-to-aspnet-identity.md) (il nuovo sistema di appartenenze per ASP.NET) a un'applicazione ASP.NET.
> 
> Quando si crea un nuovo progetto Web Form o MVC in Visual Studio 2013 RTM con singoli account, Visual Studio verrà installare tutti i pacchetti necessari e aggiungere tutte le classi necessarie per l'utente. Questa esercitazione illustrerà i passaggi per aggiungere il supporto di ASP.NET Identity per il progetto Web Form esistente o un nuovo progetto vuoto. Verranno descritti tutti i pacchetti NuGet che necessari per installare e le classi che è necessario aggiungere. Esaminerà esempio Web Form per la registrazione di nuovi utenti e la registrazione in durante l'evidenziazione di tutte le API di punto di ingresso principale per l'autenticazione e gestione degli utenti. In questo esempio userà l'implementazione predefinita di ASP.NET Identity per l'archiviazione dei dati SQL che si basa su Entity Framework. Questa esercitazione si userà LocalDB per il database SQL.
> 
> Questa esercitazione è stato scritto da Raquel Soares De Almeida e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Introduzione ad ASP.NET Identity

1. Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Fare clic su **nuovo progetto** dall'inizio pagina oppure è possibile usare il menu e selezionare **File**e quindi **nuovo progetto**.
3. Selezionare **Visual c# i** n riquadro a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Denominare il progetto "WebFormsIdentity" e quindi fare clic su **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Si noti che il **Modifica autenticazione** pulsante è disabilitato e non viene fornito alcun supporto per l'autenticazione in questo modello. I modelli Web Form, MVC e API Web consentono di selezionare l'approccio di autenticazione. Per altre informazioni, vedere [panoramica dell'autenticazione](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Aggiunta di pacchetti di identità all'App

In Esplora soluzioni fare clic sul progetto e selezionare **Gestisci pacchetti NuGet**. Nella finestra casella di testo di ricerca, digitare "*Identity.E*". Fare clic su Installa per questo pacchetto.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Si noti che questo pacchetto installerà i pacchetti di dipendenza: Entity Framework e Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Aggiunta di Web Form per registrare gli utenti

1. Nelle **Esplora soluzioni**, fare clic sul progetto e fare clic su **Add**e quindi **modulo Web**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. Nel **Specifica nome per l'elemento** della finestra di dialogo Nome nuovo web form **registrare**, quindi fare clic su **OK**
3. Sostituire il markup generato *Register* file con il codice seguente. Le modifiche al codice sono evidenziate.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Questo è solo una versione semplificata del *Register* file che viene creato quando si crea un nuovo progetto di Web Form ASP.NET. Il markup precedente aggiunge i campi del modulo e un pulsante per registrare un nuovo utente.
4. Aprire il *Register.aspx.cs* file e sostituire il contenuto del file con il codice seguente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Il codice precedente è una versione semplificata del *Register.aspx.cs* file che viene creato quando si crea un nuovo progetto di Web Form ASP.NET.
    > 2. Il *IdentityUser* classe è l'implementazione di Entity Framework predefinito delle *IUser* interfaccia. *IUser* è l'interfaccia minima per un utente in ASP.NET Identity Core.
    > 3. Il *nello UserStore* classe è l'implementazione di Entity Framework predefinito di un archivio utente. Questa classe implementa interfacce minime di ASP.NET Identity Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* e *IUserRoleStore* .
    > 4. Il *UserManager* API che salvano automaticamente le modifiche a correlate all'utente di classe espone le *nello UserStore*.
    > 5. Il *IdentityResult* classe rappresenta il risultato di un'operazione di identità.
5. Nelle **Esplora soluzioni**, fare clic sul progetto e fare clic su **Add**, **Aggiungi cartella ASP.NET** e quindi **App\_dati**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Aprire il *Web. config* file e aggiungere una voce della stringa di connessione per il database verrà usato per archiviare informazioni utente. Il database verrà creato in fase di esecuzione da Entity Framework per le entità di identità. La stringa di connessione è simile a quello creato automaticamente quando si crea un nuovo progetto di Web Form. Il codice evidenziato di seguito viene illustrato il markup che è consigliabile aggiungere:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Per Visual Studio 2015 o versione successiva, sostituire `(localdb)\v11.0` con `(localdb)\MSSQLLocalDB` nella stringa di connessione.
    
7. Pulsante destro del mouse fare clic sul file *Register* nel progetto e selezionare **imposta come pagina iniziale**. Premere CTRL+F5 per compilare ed eseguire l'applicazione web. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity include il supporto per la convalida e in questo esempio è possibile verificare il comportamento predefinito sull'utente e Password validator che provengono dal pacchetto di base di identità. La convalida predefinita per l'utente (`UserValidator`) ha una proprietà `AllowOnlyAlphanumericUserNames` con valore predefinito impostato su `true`. La convalida predefinita per la Password (`MinimumLengthValidator`) garantisce che la password è composta da almeno 6 caratteri. Queste convalide sono di proprietà in `UserManager` che può essere sottoposto a override se si desidera avere la convalida personalizzata,

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Verifica per determinare se il Database di LocalDb identità e le tabelle generate da Entity Framework

1. Nel **View** menu, fare clic su **Esplora Server**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Espandere **DefaultConnection (WebFormsIdentity)**, espandere **tabelle**, fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Configurazione dell'applicazione per l'autenticazione OWIN

A questo punto è stata aggiunta solo il supporto per la creazione di utenti. A questo punto, si intende dimostrare come è possibile aggiungere l'autenticazione per accedere a un utente. ASP.NET Identity Usa il middleware di autenticazione OWIN di Microsoft per l'autenticazione basata su form. L'autenticazione dei Cookie OWIN un cookie e dichiara come meccanismo di autenticazione basata su che può essere usato da un framework ospitato in [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) o IIS. Con questo modello, è possono utilizzare gli stessi pacchetti di autenticazione tra più Framework, tra cui MVC ASP.NET e Web Form. Per ulteriori informazioni su come eseguire middleware in un host indipendente vedere e progetto Katana [Introduzione al Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>L'installazione dei pacchetti di autenticazione all'applicazione

1. In Esplora soluzioni fare clic sul progetto e selezionare **Gestisci pacchetti NuGet**. Nella finestra casella di testo di ricerca, digitare "*Identity.Owin*". Fare clic su Installa per questo pacchetto.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Ricerca per il pacchetto ***systemweb*** e installarlo.   

    > [!NOTE]
    > Il **Microsoft.Aspnet.Identity.Owin** pacchetto contiene un set di classi dell'estensione per OWIN per gestire e configurare il middleware di autenticazione OWIN devono essere utilizzate dai pacchetti di ASP.NET Identity Core.  
    > Il **systemweb** pacchetto contiene un server OWIN che consente alle applicazioni basate su OWIN per l'esecuzione in IIS utilizzando la pipeline delle richieste ASP.NET. Per altre informazioni, vedere [Middleware OWIN in IIS integrati pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Aggiunta di classi di configurazione di autenticazione e OWIN Startup

1. Nelle **Esplora soluzioni**, fare clic sul progetto, fare clic su **Add**e quindi **Aggiungi nuovo elemento**. Nella finestra casella di testo di ricerca, digitare "*owin*". Denominare la classe "*avvio*" e fare clic su **Add**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Nel file Startup.cs, aggiungere il codice evidenziato seguente per configurare l'autenticazione tramite cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Questa classe contiene il `OwinStartup` attributo per specificare la classe di avvio OWIN. Ogni applicazione OWIN dispone di una classe di avvio in cui è specificare i componenti della pipeline dell'applicazione. Visualizzare [rilevamento della classe di avvio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) per altre informazioni su questo modello.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Aggiunta di Web Form per la registrazione e accesso degli utenti

1. Aprire il *Register.cs* file e aggiungere il codice seguente che accederà all'utente quando la registrazione ha esito positivo. Le modifiche sono evidenziate di seguito.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Poiché ASP.NET Identity e l'autenticazione tramite Cookie OWIN sono basata sulle attestazioni di sistema, il framework richiede lo sviluppatore dell'app generare una [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) per l'utente. Oggetto ClaimsIdentity offre informazioni su tutte le attestazioni dell'utente, ad esempio i ruoli a cui appartiene l'utente. È anche possibile aggiungere altre attestazioni dell'utente in questa fase.
    > - È possibile accedere all'utente tramite il AuthenticationManager da OWIN e chiamando il metodo `SignIn` e passando l'oggetto ClaimsIdentity come illustrato in precedenza. Questo codice verrà accesso dell'utente e genera anche un cookie. Questa chiamata è analoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizzato per il [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulo.
2. Nelle **Esplora soluzioni**, fare doppio clic il progetto, fare clic **Add**e quindi **modulo Web**. Denominare il web form **account di accesso**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Sostituire il contenuto del *aspx* file con il codice seguente:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Sostituire il contenuto del *Login.aspx.cs* file con il codice seguente:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Il `Page_Load` controlla lo stato dell'utente corrente e intraprende le azioni in base a questo punto relativo `Context.User.Identity.IsAuthenticated` dello stato.  
    >     **Visualizzare connesso in nome utente** : Framework di identità di Microsoft ASP.NET ha aggiunto i metodi di estensione nella [IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) che consente di ottenere il `UserName` e `UserId` per il Utente connesso. Questi metodi di estensione sono definiti nel `Microsoft.AspNet.Identity.Core` assembly. Questi metodi di estensione sono la sostituzione di [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Metodo di accesso:   
    >     `This` metodo sostituisce il precedente `CreateUser_Click` metodo in questo esempio e l'ora consente l'accesso all'utente dopo la corretta creazione l'utente.   
    >  Il Framework di OWIN di Microsoft ha aggiunto i metodi di estensione nella `System.Web.HttpContext` che consente di ottenere un riferimento a un `IOwinContext`. Questi metodi di estensione sono definiti `Microsoft.Owin.Host.SystemWeb` assembly. Il `OwinContext` classe espone un `IAuthenticationManager` proprietà che rappresenta la funzionalità middleware di autenticazione disponibile sulla richiesta corrente.  
    >  Può accedere l'utente usando il `AuthenticationManager` da OWIN e chiamando `SignIn` e passando il `ClaimsIdentity` come illustrato in precedenza.   
    >  Poiché ASP.NET Identity e l'autenticazione tramite Cookie OWIN sono basate sulle attestazioni di sistema, il framework richiede l'app genererà un `ClaimsIdentity` per l'utente.   
    >  Il `ClaimsIdentity` dispone di informazioni su tutte le attestazioni dell'utente, ad esempio i ruoli a cui appartiene l'utente. È anche possibile aggiungere altre attestazioni dell'utente in questa fase  
    >  Questo codice verrà accesso dell'utente e genera anche un cookie. Questa chiamata è analoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizzato per il [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulo.
    > - `SignOut` metodo:   
    >  Ottiene un riferimento per la `AuthenticationManager` da OWIN e chiamate `SignOut`. Questo comportamento è analogo a [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodo utilizzato per il [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulo.
5. Premere **Ctrl + F5** per compilare ed eseguire l'applicazione web. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Nota: A questo punto, il nuovo utente viene creato e registrato nel.
6. Fare clic su **disconnettersi** pulsante. Si verrà reindirizzati al registro nel modulo.
7. Immettere un nome utente non valido o password e fare clic **Accedi** pulsante.   
   Il `UserManager.Find` metodo restituirà null e il messaggio di errore: " *nome utente non valido o password* " verranno visualizzati.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
