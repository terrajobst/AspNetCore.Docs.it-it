---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Aggiunta di sicurezza e l'appartenenza a un Web ASP.NET di pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo capitolo viene illustrato come proteggere il sito Web in modo che alcune delle pagine sono disponibili solo agli utenti l'accesso. (Si verrà inoltre illustrato come creare pagine che...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 351368a356a71e85d4abfdceac8d4f84e0b217f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898859"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Aggiunta della protezione e l'appartenenza al sito Web ASP.NET (Razor) pagine
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come proteggere un sito Web ASP.NET Web Pages (Razor) in modo che alcune delle pagine sono disponibili solo agli utenti l'accesso. (Verrà anche vedere come creare pagine che chiunque può accedere.)
> 
> **Illustra quanto segue:** 
> 
> - Come creare un sito Web che è una pagina di registrazione e una pagina di accesso in modo che per alcune pagine è possibile limitare l'accesso a solo i membri.
> - Come creare pagine e solo membri pubbliche.
> - Come definire i ruoli, che sono gruppi che dispongono delle autorizzazioni di sicurezza diverso del sito, e come assegnare utenti a un ruolo.
> - Descrive come usare CAPTCHA per impedire la creazione di account membri di programmi automatizzati (componenti).
>   
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il WebMatrix **Starter Site** modello.
> - Il `WebSecurity` helper e `Roles` classe.
> - Il `ReCaptcha` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


È possibile configurare il sito Web in modo che gli utenti possono accedere al suo interno &#8212; , ovvero in modo che il sito supporta *appartenenza*. Questo può essere utile per diversi motivi. Ad esempio, il sito potrebbe essere pagine che devono essere disponibile solo per i membri. In alcuni casi, è possibile richiedere agli utenti di accedere modo da inviare commenti e suggerimenti oppure lasciare un commento.

Anche se il sito Web supporta l'appartenenza, gli utenti non sono necessariamente necessari per l'accesso prima di utilizzare alcune delle pagine nel sito. Utenti che non sono connessi sono noti come *gli utenti anonimi*.

Un utente può registrare nel sito Web e può quindi accedere al sito. Il sito Web richiede un nome utente (un indirizzo di posta elettronica) e una password per confermare che gli utenti che dichiarano di essere. Questo processo di accesso e conferma l'identità dell'utente è noto come *autenticazione*.

È possibile impostare la protezione e l'appartenenza in modi diversi:

- Se si utilizza WebMatrix, in modo semplice consiste nel creare come nuovo sito basato sul **Starter Site** modello. Questo modello è già configurato per la sicurezza e l'appartenenza e dispone già di una pagina di registrazione, una pagina di accesso e così via.

    Il sito creato tramite il modello è anche un'opzione per consentire agli utenti di accedere con un sito esterno come Facebook, Google, o Twitter.
- Se si desidera aggiungere protezione a un sito esistente o se non si desidera utilizzare il **Starter Site** modello, è possibile creare la propria pagina di registrazione, una pagina di accesso e così via.

Questo articolo illustra la prima opzione &mdash; come aggiunta di sicurezza utilizzando il **Starter Site** modello. Fornisce inoltre alcune informazioni di base sulle modalità di implementazione per la sicurezza e quindi vengono forniti collegamenti a ulteriori informazioni su come eseguire questa operazione. È inoltre informazioni su come abilitare l'account di accesso esterni, come descritto in dettaglio in un articolo separato.

## <a name="creating-website-security-using-the-starter-site-template"></a>Creazione di sicurezza del sito Web utilizzando il modello di sito Starter

In WebMatrix, è possibile utilizzare il **Starter Site** modello per creare un sito Web che contiene quanto segue:

- Un database che viene utilizzato per archiviare nomi utente e password per i membri.
- Una pagina di registrazione in cui è possono registrare gli utenti anonimi di (nuovi).
- Una pagina di accesso e di disconnessione.
- Una pagina di ripristino e la reimpostazione della password.

La procedura seguente viene descritto come creare il sito e configurarlo.

1. Avviare WebMatrix e la **avvio rapido** selezionare **del sito da un modello**.
2. Selezionare il **Starter Site** modello e quindi fare clic su **OK**. WebMatrix consente di creare un nuovo sito.
3. Nel riquadro a sinistra, fare clic su di **file** selettore dell'area di lavoro.
4. Nella cartella radice del sito Web, aprire il  *\_AppStart.cshtml* file, ovvero un file speciale a cui viene utilizzato per contenere le impostazioni globali. Contiene alcune istruzioni che sono impostate come commento tramite il `//` caratteri:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Configurare queste istruzioni la `WebMail` helper, che può essere utilizzato per inviare posta elettronica. Il sistema di appartenenze può utilizzare posta elettronica per inviare messaggi di conferma quando gli utenti registrano o quando si desidera modificare le password. (Ad esempio, dopo la registrazione, gli utenti ricevono un messaggio di posta elettronica che include un collegamento che è possibile fare clic per completare il processo di registrazione.)

    L'invio di posta elettronica richiede l'accesso a un server SMTP, come descritto in [aggiunta di posta elettronica a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202899). Archiviare le impostazioni di posta elettronica in questo centro  *\_AppStart.cshtml* file in modo che non è necessario codice ripetutamente in ogni pagina che può inviare posta elettronica. (Non è necessario configurare le impostazioni SMTP per configurare un database di registrazione, le impostazioni SMTP è necessario solo se si desidera convalidare i relativi alias di posta elettronica agli utenti e consentire agli utenti di reimpostare una password dimenticata).
5. Rimuovere le istruzioni rimuovendo `//` davanti alla ciascuno di essi.

    Se non si desidera impostare la conferma tramite posta elettronica, è possibile ignorare questo passaggio e il passaggio successivo. Se i valori SMTP non sono impostati, il nuovo account è immediatamente disponibile senza un messaggio di posta elettronica di conferma.
6. Modificare le impostazioni correlate al messaggio di posta elettronica seguenti nel codice:

   - Impostare `WebMail.SmtpServer` sul nome del server SMTP che è possibile accedere.
   - Lasciare `WebMail.EnableSsl` impostato su `true`. Questa impostazione consente di proteggere le credenziali vengono inviate al server SMTP crittografandole.
   - Impostare `WebMail.UserName` per il nome utente per l'account del server SMTP.
   - Impostare `WebMail.Password` la password per l'account del server SMTP.
   - Impostare `WebMail.From` sul proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio.

     > [!NOTE] 
     > 
     > **Suggerimento** per ulteriori informazioni sui valori per queste proprietà, vedere [configurazione delle impostazioni di posta elettronica](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) in [personalizzazione comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Salvare e chiudere  *\_AppStart.cshtml*.
8. Eseguire il *cshtml* pagina in un browser.

    ![sicurezza-appartenenza-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Se viene visualizzato un errore che indica che una proprietà deve essere un'istanza di `ExtendedMembershipProvider`, il sito potrebbe non essere configurato per utilizzare il sistema di appartenenze di ASP.NET Web Pages (SimpleMembership). In alcuni casi, ciò può verificarsi se il server di un provider di hosting è configurato in modo diverso rispetto al server locale. Per risolvere questo problema, aggiungere l'elemento seguente nel sito *Web. config* file:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Aggiungere questo elemento come figlio di `<configuration>` elemento e come peer del `<system.web>` elemento.
9. Nell'angolo superiore destro della pagina, fare clic su di **registrare** collegamento. Il *cshtml* viene visualizzata la pagina.
10. Immettere un nome utente e una password e quindi fare clic su **registrare**.

    ![sicurezza-appartenenza-3](16-adding-security-and-membership/_static/image2.png)

    Al momento della creazione del sito Web dal **Starter Site** modello, un database denominato *StarterSite.sdf* è stato creato il sito *App\_dati* cartella. Durante la registrazione, le informazioni sull'utente viene aggiunto al database. Se si impostano i valori SMTP, viene inviato un messaggio all'indirizzo di posta elettronica che è stato utilizzato in modo sarà possibile completare la registrazione.

    ![sicurezza-appartenenza-4](16-adding-security-and-membership/_static/image3.png)
11. Passare al programma di posta elettronica e cercare il messaggio, che conterrà il codice di conferma e un collegamento ipertestuale al sito.
12. Fare clic sul collegamento ipertestuale per attivare l'account. Il collegamento ipertestuale conferma apre una pagina di conferma di registrazione.

    ![sicurezza-appartenenza-5](16-adding-security-and-membership/_static/image4.png)
13. Fare clic su di **accesso** collegamento e quindi accedere utilizzando l'account che è stato registrato.

      Dopo l'accesso, il **accesso** e **registrare** collegamenti vengono sostituiti da un **Logout** collegamento. Il nome di accesso viene visualizzato come collegamento. (Il collegamento consente di passare a una pagina in cui è possibile modificare la password.)

      ![sicurezza-appartenenza-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Per impostazione predefinita, le pagine web ASP.NET inviare le credenziali al server in testo non crittografato (come testo leggibile dall'utente). Un sito di produzione deve usare HTTP protetto (https://, noto anche come il *SSL sicura* o SSL) per crittografare le informazioni riservate che vengono scambiate con il server. È possibile posta elettronica necessario inviare i messaggi tramite SSL impostando `WebMail.EnableSsl=true` come nell'esempio precedente. Per ulteriori informazioni su SSL, vedere [protezione delle comunicazioni Web: i certificati SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funzionalità di appartenenza aggiuntive nel sito

Il sito contiene altre funzionalità che consente agli utenti di gestire gli account. Gli utenti possono eseguire le operazioni seguenti:

- Modificare le password. Dopo l'accesso, è possibile fare clic il nome utente (che è un collegamento). Questa operazione viene in una pagina in cui è possibile creare una nuova password (*Account/ChangePassword.cshtml*).
- Recuperare una password dimenticata. Nella pagina di accesso, è presente un collegamento (**dimenticata la password?**) che consente agli utenti a una pagina (*Account/ForgotPassword.cshtml*) in cui è possibile immettere un indirizzo di posta elettronica. Il sito invia un messaggio di posta elettronica contenente un collegamento che è possibile fare clic per impostare una nuova password (*Account/PasswordReset.cshtml*).

È inoltre possibile consentire agli utenti possono inoltre l'accesso utilizzando un sito esterno, come illustrato in seguito.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Creazione di una pagina solo i membri

Per il momento, chiunque può visualizzare qualsiasi pagina del sito Web. Ma è possibile disporre di pagine che sono disponibili solo per gli utenti che hanno effettuato l'accesso (ovvero, ai membri). ASP.NET consente di creare pagine di cui è possibile accedere solo dai membri connesso. In genere, se gli utenti anonimi tentano di accedere a una pagina solo i membri, si reindirizza alla pagina di accesso.

In questa procedura, si creerà una cartella che conterrà le pagine che sono disponibili solo per gli utenti connessi.

1. Alla radice del sito, creare una nuova cartella. (Nella barra multifunzione, fare clic sulla freccia sotto **New** e quindi scegliere **nuova cartella**.)
2. Denominare la nuova cartella *membri*.
3. All'interno di *membri* cartella, creare una nuova pagina e di averlo denominato *MembersInformation.cshtml*.
4. Sostituire il contenuto esistente con il codice e markup seguenti:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Questo codice consente di `IsAuthenticated` proprietà del `WebSecurity` oggetto, che restituisce `true` se l'utente ha effettuato l'accesso. Se l'utente non è connesso, il codice chiama `Response.Redirect` per inviare all'utente di *cshtml* nella pagina di *Account* cartella.

    L'URL di reindirizzamento include un `returnUrl` valore stringa che utilizza query `Request.Url.LocalPath` per impostare il percorso della pagina corrente. Se si imposta la `returnUrl` valore nella stringa di query simile alla seguente (e se l'URL restituito è un percorso locale), pagina di accesso restituirà gli utenti a questa pagina dopo l'accesso.

    Il codice imposta inoltre  *\_SiteLayout.cshtml* pagina come pagina di layout. (Per altre informazioni sulle pagine di layout, vedere [creazione di un Layout coerente in siti di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Esecuzione del sito. Se si è ancora connessi, fare clic su di **Logout** pulsante nella parte superiore della pagina.
6. Nel browser, richiedere la pagina */membri/MembersInformation*. Ad esempio, l'URL potrebbe essere simile al seguente:

    `http://localhost:38366/Members/MembersInformation`

    (Il numero di porta (38366) probabilmente saranno diverso nell'URL.)

    Si viene reindirizzati al *cshtml* perché non è stato effettuato pagina.
7. Accedere utilizzando l'account creato in precedenza. Si viene reindirizzati al *MembersInformation* pagina. Poiché si è connessi, questa volta verrà visualizzato il contenuto della pagina.

Per proteggere l'accesso a più pagine, è possibile farlo:

- Aggiungere il controllo della sicurezza in ogni pagina.
- Creare un  *\_Pagestart* pagina della cartella in cui mantenere le pagine protette e aggiungere il controllo di sicurezza non esiste. Il  *\_Pagestart* pagina agisce come un tipo di pagina globale per tutte le pagine nella cartella. Questa tecnica è illustrata in dettaglio in [personalizzazione di un comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Creazione di sicurezza per i gruppi di utenti (ruoli)

Se il sito ha molti membri, non è efficiente per verificare le autorizzazioni per ogni utente singolarmente prima che una pagina. In alternativa è possibile eseguire consiste nel creare gruppi, o *ruoli*, che i singoli membri appartengono a. È quindi possibile controllare le autorizzazioni in base al ruolo. In questa sezione si creerà un &quot;admin&quot; ruolo e quindi creare una pagina che sia accessibile agli utenti che sono in (che appartengono ai) tale ruolo.

Il sistema di appartenenze ASP.NET è configurare per supportare i ruoli. Tuttavia, a differenza di registrazione di appartenenza e l'account di accesso, il **Starter Site** modello non contiene pagine che consentono di gestire i ruoli. (Gestione dei ruoli è un'attività amministrativa anziché un'attività definita dall'utente). Tuttavia, è possibile aggiungere gruppi direttamente nel database delle appartenenze in WebMatrix.

1. In WebMatrix, fare clic su di **database** selettore dell'area di lavoro.
2. Nel riquadro a sinistra, aprire il *StarterSite.sdf* nodo, aprire il **tabelle** nodo, quindi fare doppio clic il *pagine Web\_ruoli* tabella.

    ![sicurezza-appartenenza-7](16-adding-security-and-membership/_static/image6.png)
3. Aggiungere un ruolo denominato &quot;admin&quot;. Il *RoleId* campo viene compilato automaticamente. (È la chiave primaria ed è stata impostata per essere un campo di identificazione, come illustrato in [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Prendere nota di ciò che è il valore per il *RoleId* campo. (Se questo è il primo ruolo che si sta definendo, sarà 1.)

    ![sicurezza-appartenenza-8](16-adding-security-and-membership/_static/image7.png)
5. Chiudi il *pagine Web\_ruoli* tabella.
6. Aprire il *UserProfile* tabella.
7. Annotare il *UserId* valore di uno o più utenti nella tabella e quindi chiudere la tabella.
8. Aprire il *pagine Web\_UserInRoles* tabella e immettere un *UserID* e *RoleID* valore nella tabella. Ad esempio, per inserire l'utente 2 nel &quot;admin&quot; ruolo, immettere questi valori:

    ![sicurezza-appartenenza-9](16-adding-security-and-membership/_static/image8.png)
9. Chiudi il *pagine Web\_UsersInRoles* tabella.

    Dopo aver creato i ruoli definiti, è possibile configurare una pagina che sia accessibile agli utenti che fanno parte di tale ruolo.
10. Nella cartella radice del sito Web, creare una nuova pagina denominata *AdminError.cshtml* e sostituire il contenuto esistente con il codice seguente. Questa sarà la pagina che gli utenti vengono reindirizzati alla se essi non sono autorizzati ad accedere a una pagina.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Nella cartella radice del sito Web, creare una nuova pagina denominata *AdminOnly.cshtml* e sostituire il codice esistente con il codice seguente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Il `Roles.IsUserInRole` restituisce `true` se l'utente corrente è un membro del ruolo specificato (in questo caso, il ruolo "admin").
12. Eseguire *cshtml* in un browser, ma non accedi. (Se si è già connessi, la disconnessione.)
13. Nella barra degli indirizzi del browser aggiungere *AdminOnly* nell'URL. (In altre parole, è richiesta la *AdminOnly.cshtml* file.) Si viene reindirizzati al *AdminError.cshtml* pagina, poiché non attualmente effettuato l'accesso come utente di &quot;admin&quot; ruolo.
14. Tornare alla *cshtml* e accedere come l'utente è stato aggiunto per il &quot;admin&quot; ruolo.
15. Passare a *AdminOnly.cshtml* pagina. Questa volta, viene visualizzata la pagina.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impedisce l'aggiunta del sito Web di programmi automatizzati

Pagina di accesso non comporta l'arresto programmi automatizzati (talvolta detto *web robot* o *BOT*) tramite la registrazione con il sito Web. Questa procedura viene descritto come consentire a un test di ReCaptcha per la pagina di registrazione.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrare il sito Web in ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Dopo aver completato la registrazione, si otterrà una chiave pubblica e una chiave privata.
2. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.
3. Nel *Account* cartella, aprire il file denominato *cshtml*.
4. Nel codice nella parte superiore della pagina, trovare le righe seguenti e rimuovere i commenti li rimuovendo il `//` caratteri di commento:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Sostituire `PRIVATE_KEY` con la propria chiave privata di ReCaptcha.
6. Nel markup della pagina, rimuovere il `@*` e `*@` commento da racchiudere le righe seguenti nel markup della pagina:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Sostituire `PUBLIC_KEY` con la chiave.
8. Se è ancora stato rimosso già, rimuovere il `<div>` elemento che contiene il testo che inizia con "Per abilitare la verifica CAPTCHA...". (Rimuovere l'intera `<div>` elemento e il relativo contenuto.)

9. Eseguire *cshtml* in un browser. Se si è connessi al sito, fare clic su di **Logout** collegamento.
10. Fare clic su di **registrare** collegamento e i test la registrazione tramite il test CAPTCHA.

     ![security-membership-10](16-adding-security-and-membership/_static/image9.png)

Per ulteriori informazioni sul `ReCaptcha` supporto, vedere [utilizzando un CATPCHA per evitare che programmi automatizzata (Bot) da utilizzando il sito Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Agli utenti di accedere con un sito esterno

Il **Starter Site** modello include codice e markup che consente agli utenti di accedere con Facebook, Windows Live, Twitter, Google o Yahoo. Per impostazione predefinita, questa funzionalità non è abilitata. La procedura generale per l'utilizzo agli utenti di consentire l'accesso utilizzando questi provider esterni è:

- Decidere quali siti esterni che si desidera supportare.
- Se necessario, passare a tale sito e configurare un'app di account di accesso. (Ad esempio, è necessario eseguire questa operazione per consentire gli account di accesso di Facebook.)
- Nel sito, configurare il provider. Nella maggior parte dei casi, è sufficiente rimuovere il commento del codice nel  *\_AppStart.cshtml* file.
- Aggiungere il markup alla pagina di registrazione che consente di collegamento al sito per l'accesso esterno. In genere, è possibile copiare il codice sia necessario modificare leggermente il testo.

È possibile trovare istruzioni dettagliate nell'argomento [abilitazione di account di accesso da siti esterni in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969).

Dopo che un utente accede da un altro sito, l'utente torna al sito e *associa* tale account di accesso con il sito. In effetti, ciò crea una voce di appartenenza del sito per l'account di accesso dell'utente esterno. Ciò consente di utilizzare le funzionalità normale di appartenenza (ad esempio i ruoli) con accesso esterno.

## <a name="adding-security-to-an-existing-website"></a>Aggiunta della protezione a un sito Web esistente

La procedura più indietro in questo articolo si basa sull'utilizzo di **Starter Site** modello come base per la sicurezza del sito Web. Se non è pratico per iniziare dal **Starter Site** modello o per copiare le pagine rilevanti da un sito basato su tale modello, è possibile implementare lo stesso tipo di sicurezza in un sito utilizzando il codice manualmente per. Creare gli stessi tipi di pagine, registrazione, l'account di accesso e così via e quindi usare classi e gli helper per configurare l'appartenenza.

Il processo di base è descritto nel post di blog [il modo più semplice per implementare la sicurezza ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). La maggior parte delle operazioni viene eseguita utilizzando i seguenti metodi e proprietà del `WebSecurity` helper:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Questi metodi consentono di determinare se un utente è già registrato e per registrarli.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Questa proprietà consente di determinare se l'utente corrente è connesso. Ciò è utile per reindirizzare gli utenti a una pagina di accesso se non è ancora effettuato l'accesso.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Questi metodi di accesso di un utente in o out.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Questa proprietà è utile per la visualizzazione connesso nome dell'utente corrente (se l'utente è connesso).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Questo metodo è utile se è impostata una conferma tramite posta elettronica per la registrazione. (I dettagli sono descritti nel post di blog [utilizzando la funzionalità di conferma per la sicurezza ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Per gestire i ruoli, è possibile utilizzare il [ruoli](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) e [appartenenza](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) classi, come descritto nel post di blog.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Personalizzazione del comportamento a livello di sito](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Protezione delle comunicazioni Web: I certificati SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [IL modo più semplice per implementare la sicurezza ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) e [utilizzano la funzionalità di conferma per la sicurezza ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Si tratta di post di blog che descrivono come implementare la funzionalità di appartenenza ASP.NET senza utilizzare il **Starter Site** modello.
- [Abilitazione dell'accesso da siti esterni in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Riferimento all'API di classe WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Riferimento all'API di classe SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Riferimento all'API di classe SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
