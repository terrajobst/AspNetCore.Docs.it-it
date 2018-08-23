---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Aggiunta di sicurezza e l'appartenenza a un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: In questo capitolo illustra come proteggere il sito Web in modo che alcune delle pagine sono disponibili solo a utenti che accedono. (Verrà anche illustrato come creare pagine che...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: ae574706ecd14f1cafdb2d8b6340477e50246a32
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836526"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>L'aggiunta della protezione e l'appartenenza a un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come proteggere un sito Web di ASP.NET Web Pages (Razor) in modo che alcune delle pagine sono disponibili solo a utenti che accedono. (Verrà anche illustrato come creare pagine che chiunque può accedere.)
> 
> **Che cosa si apprenderà come:** 
> 
> - Come creare un sito Web che dispone di una pagina di registrazione e una pagina di accesso in modo che per alcune tabelle è possibile limitare l'accesso solo agli utenti membri.
> - Come creare pagine e riservate ai membri pubbliche.
> - Come definire i ruoli, che sono gruppi che dispongono delle autorizzazioni di sicurezza diverso nel sito, e come assegnare gli utenti a un ruolo.
> - Come usare CAPTCHA per impedire la creazione di account membri di programmi automatizzati (Bot).
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
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


È possibile configurare il sito Web in modo che gli utenti possono accedere al suo interno &#8212; , ovvero in modo che il sito supporta *appartenenza*. Ciò può essere utile per diversi motivi. Ad esempio, il sito potrebbe essere pagine che devono essere disponibili solo per i membri. In alcuni casi, è possibile richiedere agli utenti di accedere per inviare commenti o lasciare un commento.

Anche se il sito Web supporta l'appartenenza, gli utenti non sono necessariamente necessari per accedere prima di usare alcune delle pagine nel sito. Gli utenti che non sono connessi sono noti come *gli utenti anonimi*.

Un utente può registrare nel tuo sito Web e quindi possibile accedere al sito. Il sito Web richiede un nome utente (un indirizzo di posta elettronica) e una password per confermare che gli utenti siano effettivamente chi dichiara di essere. Questo processo di accesso e conferma l'identità dell'utente è noto come *autenticazione*.

È possibile impostare la sicurezza e l'appartenenza in modi diversi:

- Se si utilizza WebMatrix, in modo semplice consiste nel creare come nuovo sito basato sul **Starter Site** modello. Questo modello è già configurato per la sicurezza e l'appartenenza e dispone già di una pagina di registrazione, una pagina di accesso e così via.

    Il sito creato tramite il modello include anche un'opzione per consentire agli utenti di accedere con un sito esterno, ad esempio Facebook, Google o Twitter.
- Se si desidera incrementare la sicurezza a un sito esistente o se non si vuole usare il **Starter Site** modello, è possibile creare la propria pagina di registrazione, pagina di accesso e così via.

Questo articolo illustra la prima opzione &mdash; come incrementare la sicurezza usando il **Starter Site** modello. Fornisce inoltre alcune informazioni di base su come implementare la protezione e quindi vengono forniti collegamenti a ulteriori informazioni su come eseguire questa operazione. È anche informazioni su come abilitare gli account di accesso esterno, che è descritti in dettaglio in un articolo separato.

## <a name="creating-website-security-using-the-starter-site-template"></a>Creazione di sicurezza del sito Web usando il modello di sito Starter

In WebMatrix, è possibile usare la **Starter Site** modello per creare un sito Web che contiene gli elementi seguenti:

- Un database che viene usato per archiviare nomi utente e password per i membri.
- Una pagina di registrazione in cui è possono registrare utenti anonimi (nuovi).
- Una pagina di accesso e disconnessione.
- Una pagina di ripristino e la reimpostazione della password.

La procedura seguente viene descritto come creare il sito e configurarlo.

1. Avviare WebMatrix e la **avvio rapido** pagina, selezionare **sito da modello**.
2. Selezionare il **Starter Site** modello e quindi fare clic su **OK**. WebMatrix consente di creare un nuovo sito.
3. Nel riquadro a sinistra, scegliere il **file** selettore dell'area di lavoro.
4. Nella cartella radice del sito Web, aprire il  *\_AppStart.cshtml* file, ovvero un file speciale che viene usato per contenere le impostazioni globali. Contiene alcune istruzioni che vengano impostata come commento tramite il `//` caratteri:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Configurare queste istruzioni di `WebMail` helper, che può essere usato per inviare posta elettronica. Il sistema di appartenenze può utilizzare posta elettronica per inviare messaggi di conferma quando gli utenti registrano o quando si vogliono modificare le password. (Ad esempio, dopo la registrazione, gli utenti ricevono un messaggio di posta elettronica che include un collegamento che possono fare clic per terminare il processo di registrazione.)

    L'invio di posta elettronica richiede l'accesso a un server SMTP, come descritto in [aggiunta di posta elettronica a un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202899). Si archivierà le impostazioni di posta elettronica in questo central  *\_AppStart.cshtml* file in modo che non è necessario scrivere codice in essi ripetutamente in ogni pagina che può inviare posta elettronica. (Non è necessario configurare le impostazioni SMTP per configurare un database di registrazione; occorre solo le impostazioni SMTP se si desidera convalidare gli utenti da loro alias di posta elettronica e consentire agli utenti di reimpostare una password dimenticata).
5. Rimuovere le istruzioni rimuovendo `//` davanti alla ognuno di essi.

    Se non vuoi configurare conferma tramite posta elettronica, è possibile ignorare questo passaggio e il passaggio successivo. Se i valori SMTP non sono impostati, il nuovo account è immediatamente disponibile senza un messaggio di posta elettronica di conferma.
6. Modificare le impostazioni seguenti relative alla posta elettronica nel codice:

   - Impostare `WebMail.SmtpServer` sul nome del server SMTP che è possibile utilizzare.
   - Lasciare `WebMail.EnableSsl` impostato su `true`. Questa impostazione consente di proteggere le credenziali vengono inviate al server SMTP crittografandole.
   - Impostare `WebMail.UserName` per il nome utente per l'account del server SMTP.
   - Impostare `WebMail.Password` alla password per l'account del server SMTP.
   - Impostare `WebMail.From` per il proprio indirizzo di posta elettronica. Questo è il messaggio viene inviato dall'indirizzo di posta elettronica.

     > [!NOTE] 
     > 
     > **Suggerimento** per altre informazioni sui valori per queste proprietà, vedere [configurazione delle impostazioni di posta elettronica](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) nelle [personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Salvare e chiudere  *\_AppStart.cshtml*.
8. Eseguire la *cshtml* pagina in un browser.

    ![Security-membership-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Se viene visualizzato un errore che indica che una proprietà deve essere un'istanza di `ExtendedMembershipProvider`, il sito potrebbe non essere configurato per usare il sistema di appartenenze ASP.NET Web Pages (SimpleMembership). In alcuni casi, ciò può verificarsi se server di un provider di hosting è configurato in modo diverso rispetto al server locale. Per risolvere questo problema, aggiungere l'elemento seguente del sito *Web. config* file:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Aggiungere questo elemento come figlio di `<configuration>` elemento e come peer della `<system.web>` elemento.
9. Nell'angolo superiore destro della pagina, scegliere il **registrare** collegamento. Il *Register* viene visualizzata la pagina.
10. Immettere un nome utente e una password e quindi fare clic su **registrare**.

    ![Security-membership-3](16-adding-security-and-membership/_static/image2.png)

    Quando è stato creato il sito Web dal **Starter Site** modello, un database denominato *StarterSite.sdf* creata con il sito *App\_dati* cartella. Durante la registrazione, le informazioni sull'utente viene aggiunto al database. Se si impostano i valori SMTP, un messaggio viene inviato all'indirizzo di posta elettronica che è stato utilizzato in modo che è possibile completare la registrazione.

    ![Security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. Passare al programma di posta elettronica e cercare il messaggio, che conterrà il codice di conferma e un collegamento ipertestuale al sito.
12. Fare clic sul collegamento ipertestuale per attivare l'account. Il collegamento ipertestuale conferma apre una pagina di conferma della registrazione.

    ![Security-membership-5](16-adding-security-and-membership/_static/image4.png)
13. Scegliere il **account di accesso** collegamento e quindi accedere con l'account che sono state registrate.

      Dopo l'accesso, il **account di accesso** e **registrare** collegamenti vengono sostituiti da un **Logout** collegamento. Il nome di accesso viene visualizzato come collegamento. (Il collegamento consente di passare a una pagina in cui è possibile modificare la password.)

      ![Security-membership-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Per impostazione predefinita, ASP.NET web pages inviano credenziali al server come testo non crittografato (come testo leggibile dall'utente). Un sito di produzione deve usare HTTP sicure (https://, noto anche come il *livello di secure socket* o SSL) per crittografare informazioni riservate che vengono scambiate con il server. È possibile messaggio di posta elettronica obbligatorio i messaggi vengano inviati tramite SSL impostando `WebMail.EnableSsl=true` come nell'esempio precedente. Per altre informazioni su SSL, vedere [protezione delle comunicazioni Web: i certificati SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funzionalità di appartenenza aggiuntivi nel sito

Il sito contiene altre funzionalità che consente agli utenti di gestire i propri account. Gli utenti possono eseguire le operazioni seguenti:

- Modificare le password. Dopo l'accesso, è possibile fare clic il nome utente (che è un collegamento). Ciò li ricava a una pagina in cui è possibile creare una nuova password (*Account/ChangePassword.cshtml*).
- Recuperare una password dimenticata. Nella pagina di accesso, è presente un collegamento (**ha si dimentica la password?**) che richiede agli utenti di una pagina (*Account/ForgotPassword.cshtml*) in cui è possibile immettere un indirizzo di posta elettronica. Il sito invia un messaggio di posta elettronica con un collegamento che possono fare clic per impostare una nuova password (*Account/PasswordReset.cshtml*).

È anche possibile consentire agli utenti possono anche accedere utilizzando un sito esterno, come illustrato in seguito.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Creazione di una pagina solo i membri

Per il momento, chiunque può esplorare le pagine nel sito Web. Ma si potrebbe voler presentano pagine che sono disponibili solo a persone che hanno effettuato l'accesso (vale a dire, ai membri). ASP.NET consente di creare pagine che sono accessibili solo dai membri ha eseguito l'accesso. In genere, se gli utenti anonimi tentano di accedere a una pagina solo i membri, si reindirizzarli alla pagina di accesso.

In questa procedura si creerà una cartella che conterrà le pagine che sono disponibili solo per gli utenti connessi.

1. Nella radice del sito, creare una nuova cartella. (Nella barra multifunzione, fare clic sulla freccia sotto **New** e quindi scegliere **nuova cartella**.)
2. Denominare la nuova cartella *membri*.
3. All'interno di *membri* cartella, creare una nuova pagina e averlo denominato *MembersInformation.cshtml*.
4. Sostituire il contenuto esistente con il codice e il markup seguente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Questo codice verifica il `IsAuthenticated` proprietà del `WebSecurity` object, che restituisce `true` se l'utente ha effettuato l'accesso. Se l'utente non è connesso, il codice chiama `Response.Redirect` per inviare all'utente il *cshtml* nella pagina il *Account* cartella.

    L'URL di reindirizzamento include un `returnUrl` valore stringa che utilizza query `Request.Url.LocalPath` per impostare il percorso della pagina corrente. Se si imposta la `returnUrl` valore nella stringa di query simile alla seguente (e se l'URL restituito è un percorso locale), la pagina di accesso restituirà gli utenti a questa pagina dopo l'accesso.

    Il codice imposta inoltre  *\_SiteLayout.cshtml* pagina come relativa pagina di layout. (Per altre informazioni sulle pagine di layout, vedere [creazione di un Layout coerente nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Esecuzione del sito. Se si è ancora connessi, fare clic sui **Logout** nella parte superiore della pagina.
6. Nel browser, richiedere la pagina */membri/MembersInformation*. Ad esempio, l'URL potrebbe essere simile al seguente:

    `http://localhost:38366/Members/MembersInformation`

    (Il numero di porta (38366) probabilmente sarà diverso nell'URL.)

    Si verrà reindirizzati per il *cshtml* pagina, perché non è stato effettuato.
7. Accedere usando l'account creato in precedenza. Si verrà reindirizzati al *MembersInformation* pagina. Poiché si è connessi, questa volta verrà visualizzato il contenuto della pagina.

Per proteggere l'accesso a più pagine, è possibile eseguire questa operazione:

- Aggiungere il controllo di sicurezza in ogni pagina.
- Creare un  *\_Pagestart* pagina nella cartella in cui mantenere le pagine protette e aggiungere il controllo di sicurezza non esiste. Il  *\_Pagestart* pagina agisce come un tipo di pagina globale per tutte le pagine nella cartella. Questa tecnica viene illustrata più dettagliatamente [personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Creazione di sicurezza per i gruppi di utenti (ruoli)

Se il sito ha molti membri, non è efficiente per controllare le autorizzazioni per ogni utente singolarmente prima di consentire loro visualizzata una pagina. Possiamo invece consiste nel creare i gruppi, o *ruoli*, che appartengono i singoli membri. È quindi possibile controllare le autorizzazioni in base al ruolo. In questa sezione si creerà un' &quot;admin&quot; ruolo e quindi creare una pagina che è accessibile agli utenti che sono in (che appartengono ai) tale ruolo.

Il sistema di appartenenze ASP.NET è configurato per supportare i ruoli. Tuttavia, a differenza di registrazione di appartenenza e account di accesso, il **Starter Site** modello non contiene pagine che consentono di gestire i ruoli. (Gestione dei ruoli è un'attività amministrativa piuttosto che un'attività definita dall'utente). Tuttavia, è possibile aggiungere gruppi direttamente nel database delle appartenenze in WebMatrix.

1. In WebMatrix, fare clic sui **database** selettore dell'area di lavoro.
2. Nel riquadro sinistro, aprire il *StarterSite.sdf* nodo, aprire il **tabelle** nodo e quindi fare doppio clic il *pagine Web\_ruoli* tabella.

    ![Security-membership-7](16-adding-security-and-membership/_static/image6.png)
3. Aggiungere un ruolo denominato &quot;admin&quot;. Il *RoleId* campo viene inserito automaticamente. (È la chiave primaria e configurata per essere un campo di identità, come illustrato in [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Prendere nota di ciò che è il valore per il *RoleId* campo. (Se si tratta del primo ruolo che si definiscono, sarà 1.)

    ![Security-membership-8](16-adding-security-and-membership/_static/image7.png)
5. Chiudi il *pagine Web\_ruoli* tabella.
6. Aprire il *profiloutente* tabella.
7. Annotare il *UserId* valore di uno o più degli utenti nella tabella e quindi chiudere la tabella.
8. Aprire il *pagine Web\_UserInRoles* tabella e immettere un *UserID* e una *RoleID* valore nella tabella. Ad esempio, per mettere l'utente 2 nel &quot;admin&quot; ruolo, è necessario immettere questi valori:

    ![Security-membership-9](16-adding-security-and-membership/_static/image8.png)
9. Chiudi il *pagine Web\_UsersInRoles* tabella.

    Dopo aver creato i ruoli definiti, è possibile configurare una pagina che è accessibile agli utenti che fanno parte di tale ruolo.
10. Nella cartella radice del sito Web, creare una nuova pagina denominata *AdminError.cshtml* e sostituire il contenuto esistente con il codice seguente. Questa sarà la pagina che gli utenti vengono reindirizzati a se essi non sono autorizzati ad accedere a una pagina.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Nella cartella radice del sito Web, creare una nuova pagina denominata *AdminOnly.cshtml* e sostituire il codice esistente con il codice seguente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Il `Roles.IsUserInRole` restituzione del metodo `true` se l'utente corrente è un membro del ruolo specificato (in questo caso, il ruolo "amministratore").
12. Eseguire *cshtml* in un browser, ma non effettuare l'accesso. (Se è già connessi, disconnettersi.)
13. Nella barra degli indirizzi del browser, aggiungere *AdminOnly* nell'URL. (In altre parole, richiedere il *AdminOnly.cshtml* file.) Si verrà reindirizzati al *AdminError.cshtml* pagina, perché non sono attualmente eseguito l'accesso come utente nel &quot;admin&quot; ruolo.
14. Tornare alla *default. cshtml* e accedere come l'utente è stato aggiunto per il &quot;admin&quot; ruolo.
15. Passare a *AdminOnly.cshtml* pagina. Questa volta viene visualizzata la pagina.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impedisce l'aggiunta del sito Web di programmi automatizzati

La pagina di accesso non interromperà i programmi automatizzati (talvolta detta *web robot* oppure *BOT*) dalla registrazione con il sito Web. Questa procedura descrive come abilitare un test di ReCaptcha per la pagina di registrazione.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrare il sito Web all'indirizzo ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Dopo aver completato la registrazione, si otterrà una chiave pubblica e una chiave privata.
2. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.
3. Nel *conto* cartella, aprire il file denominato *Register*.
4. Nel codice nella parte superiore della pagina, individuare le righe seguenti e rimuovere il commento li rimuovendo il `//` i caratteri di commento:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Sostituire `PRIVATE_KEY` con la propria chiave privata ReCaptcha.
6. Nel markup della pagina, rimuovere il `@*` e `*@` commento da tutto le righe seguenti nel markup della pagina:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Sostituire `PUBLIC_KEY` con la chiave.
8. Se è ancora stato rimosso già, rimuovere il `<div>` elemento che contiene il testo che inizia con "Per abilitare la verifica CAPTCHA...". (Rimuovere l'intera `<div>` elemento e il relativo contenuto.)

9. Eseguire *cshtml* in un browser. Se si è connessi al sito, scegliere il **Logout** collegamento.
10. Scegliere il **registrare** collegamento e i test la registrazione usando il test CAPTCHA.

     ![Security-membership-10](16-adding-security-and-membership/_static/image9.png)

Per altre informazioni sul `ReCaptcha` helper, vedere [usando un CATPCHA per evitare che programmi automatizzati (Bot) dall'uso del sito Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Consentire agli utenti di accedere con un sito esterno

Il **Starter Site** modello include codice e markup che consente agli utenti l'accesso con Facebook, Windows Live, Twitter, Google o Yahoo. Per impostazione predefinita, questa funzionalità non è abilitata. La procedura generale per l'uso di utenti consentendo agli sviluppatori l'accesso mediante questi provider esterni è questo:

- Decidere quali i siti esterni si desidera supportare.
- Se necessario, passare a tale sito e configurare un'app di account di accesso. (Ad esempio, è necessario eseguire questa operazione per consentire l'accesso con Facebook.)
- Nel sito, configurare il provider. Nella maggior parte dei casi, è sufficiente rimuovere il commento nel codice il  *\_AppStart.cshtml* file.
- Aggiungere tag alla pagina di registrazione che consente agli utenti di collegamento al sito esterno per l'accesso. In genere, è possibile copiare il codice è necessario e modificare leggermente il testo.

È possibile trovare istruzioni dettagliate nell'argomento [abilitazione di account di accesso da siti esterni in un sito di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Dopo che un utente accede da un altro sito, l'utente torna al sito e *associa* tale account di accesso con il sito. In effetti, questo crea una voce di appartenenza del sito per l'account di accesso dell'utente esterno. Ciò consente di usare le normali funzionalità di appartenenza (ad esempio, ruoli) con l'account di accesso esterno.

## <a name="adding-security-to-an-existing-website"></a>Aggiungere la protezione in un sito Web esistente

La procedura descritta in precedenza in questo articolo si basa sull'utilizzo di **Starter Site** modello come base per la sicurezza del sito Web. Se non è pratico per avviare dal **Starter Site** modello o per copiare le pagine rilevanti da un sito basato su tale modello, è possibile implementare lo stesso tipo di sicurezza nel proprio sito mediante la codifica manualmente. Creare gli stessi tipi di pagine, registrazione, accesso e così via e quindi usare classi e gli helper per configurare l'appartenenza.

Il processo di base è descritto nel post di blog [il modo più semplice per implementare la sicurezza ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). La maggior parte delle operazioni viene eseguita usando i metodi e proprietà dei seguenti il `WebSecurity` helper:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Questi metodi consentono di determinare se un utente è già registrato e registrarli.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Questa proprietà consente di determinare se l'utente corrente è connesso. Ciò è utile per reindirizzare gli utenti a una pagina di accesso se non è già effettuato l'accesso.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Questi metodi di accesso di un utente in o out.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Questa proprietà è utile per la visualizzazione connesso nome dell'utente corrente (se l'utente è connesso).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Questo metodo è utile se è impostata una conferma tramite posta elettronica per la registrazione. (I dettagli sono descritti nel post di blog [usando la funzionalità di conferma per la sicurezza di ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Per gestire i ruoli, è possibile usare la [ruoli](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) e [appartenenza](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) classi, come descritto nella voce del blog.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Personalizzazione del comportamento a livello di sito](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Proteggere le comunicazioni Web: I certificati SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [IL modo più semplice per implementare la sicurezza ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) e [usando la funzionalità di conferma per la sicurezza di ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Si tratta di post di blog che descrivono come implementare le funzionalità di appartenenza ASP.NET senza usare la **Starter Site** modello.
- [Abilitazione dell'accesso da siti esterni in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Riferimento all'API di classe WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Riferimento all'API di classe SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Riferimento all'API di classe SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
