---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: L'appartenenza e amministrazione | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: a10dbfe1ca49baee1604aac8dd9a1f93ccfcb7f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="membership-and-administration"></a>L'appartenenza e l'amministrazione
====================
Da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione viene illustrato come aggiornare l'applicazione di esempio Wingtip Toys per aggiungere un ruolo personalizzato e utilizzare ASP.NET Identity. Viene inoltre illustrato come implementare una pagina di amministrazione da cui l'utente con un ruolo personalizzato è possibile aggiungere e rimuovere i prodotti dal sito Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) è il sistema di appartenenze utilizzato per compilare un'applicazione web ASP.NET ed è disponibile in ASP.NET 4.5. Identità di ASP.NET viene utilizzato il modello di progetto di Web Form di Visual Studio 2013, nonché i modelli per [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), e [applicazione a pagina singola ASP.NET](../../../../single-page-application/index.md). È anche possibile installare il sistema di identità ASP.NET mediante NuGet quando si inizia con un'applicazione Web vuota. Tuttavia, in questa serie di esercitazioni è utilizzare il **Web Form**projecttemplate, che include il sistema di identità di ASP.NET. ASP.NET Identity rende più facile da integrare dati di profilo per ogni utente con dati dell'applicazione. ASP.NET Identity, inoltre, consente di scegliere il modello di persistenza per i profili utente nell'applicazione. È possibile archiviare i dati in un database di SQL Server o un altro archivio dati, tra cui *NoSQL* archivi dati, ad esempio tabelle di archiviazione Windows Azure.

In questa esercitazione si basa sull'esercitazione precedente denominata "Checkpoint e pagamento PayPal" della serie di esercitazioni Wingtip Toys.

## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come usare il codice per aggiungere un ruolo personalizzato e un utente all'applicazione.
- Come limitare l'accesso alla cartella di amministrazione e pagina.
- Modalità di esplorazione per l'utente che appartiene al ruolo personalizzato.
- Come utilizzare l'associazione di modelli per popolare un [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) controllo con categorie di prodotti.
- Come caricare un file dell'applicazione web mediante il [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) controllo.
- Come utilizzare i controlli di convalida per implementare la convalida dell'input.
- Come aggiungere e rimuovere i prodotti dall'applicazione.

## <a name="these-features-are-included-in-the-tutorial"></a>Queste funzionalità sono inclusi nell'esercitazione:

- Identità ASP.NET
- Configurazione e l'autorizzazione
- Associazione di modelli
- Convalida non intrusiva

Web Form ASP.NET fornisce funzionalità di appartenenza. Tramite il modello predefinito, si ottengono funzionalità di appartenenze predefinito che è possibile utilizzare immediatamente quando viene eseguita l'applicazione. In questa esercitazione viene illustrato come utilizzare ASP.NET Identity per aggiungere un ruolo personalizzato e assegnare un utente a tale ruolo. Si apprenderà come limitare l'accesso alla cartella di amministrazione. Aggiungere una pagina nella cartella di amministrazione che consente a un utente con un ruolo personalizzato per aggiungere e rimuovere i prodotti e visualizzare l'anteprima di un prodotto dopo che è stato aggiunto.

## <a name="adding-a-custom-role"></a>Aggiunta di un ruolo personalizzato

Utilizzo di ASP.NET Identity, è possibile aggiungere un ruolo personalizzato e assegnare un utente a tale ruolo tramite codice.

1. In **Esplora**, fare clic su di *logica* cartella e creare una nuova classe.
2. Denominare la nuova classe *RoleActions.cs*.
3. Modificare il codice in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. In **Esplora**, aprire il *Global.asax.cs* file.
5. Modificare il *Global.asax.cs* file aggiungendo il codice evidenziato in giallo, in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Si noti che `AddUserAndRole` è sottolineato in rosso. Fare doppio clic sul codice AddUserAndRole.  
 La lettera "A" all'inizio del metodo evidenziato apparirà sottolineata.
7. Passare il mouse sulla lettera "A" e scegliere l'interfaccia utente che consente di generare uno stub di metodo per il `AddUserAndRole` metodo. 

    ![L'appartenenza e Advministration - genera Stub metodo](membership-and-administration/_static/image1.png)
8. Scegliere l'opzione denominata:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Aprire il *RoleActions.cs* file dal *logica* cartella.  
 Il `AddUserAndRole` metodo è stato aggiunto al file di classe.
10. Modificare il *RoleActions.cs* file rimuovendo il `NotImplementedeException` e aggiungere il codice evidenziato in giallo, in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Il codice sopra riportato innanzitutto stabilisce un contesto di database per il database delle appartenenze. Il database di appartenenza viene archiviato anche un *con estensione mdf* file nel *App\_dati* cartella. Sarà in grado di visualizzare il database dopo il primo utente ha effettuato l'accesso a questa applicazione web. 

> [!NOTE] 
> 
> Se si desidera archiviare i dati di appartenenza con i dati di prodotto, è possibile utilizzare lo stesso **DbContext** utilizzata per archiviare i dati di prodotto nel codice precedente.


 Il *interno* (parola chiave) è un modificatore di accesso per i tipi (ad esempio, classi) e i membri dei tipi (ad esempio metodi o proprietà). Tipi interni o i membri sono accessibili solo all'interno di file contenuti nello stesso assembly *(con estensione dll* file). Quando si compila l'applicazione, un file di assembly *(con estensione dll*) viene creato che contiene il codice che viene eseguito quando si esegue l'applicazione. 

Oggetto `RoleStore` oggetto che fornisce la gestione dei ruoli, viene creato in base al contesto di database.

> [!NOTE] 
> 
> Si noti che quando il `RoleStore` viene creato l'oggetto viene utilizzato un oggetto generico `IdentityRole` tipo. Ciò significa che il `RoleStore` è consentita solo per contenere `IdentityRole` oggetti. Anche con i Generics, le risorse in memoria vengono gestite meglio.


Successivamente, il `RoleManager` oggetto, viene creato in base il `RoleStore` oggetto appena creato. il `RoleManager` ruolo di oggetto espone API correlate che consente di salvare automaticamente le modifiche al `RoleStore`. Il `RoleManager` è consentita solo per contenere `IdentityRole` oggetti perché il codice Usa il `<IdentityRole>` tipo generico.

Chiamare il `RoleExists` metodo per determinare se il ruolo "canEdit" è presente nel database delle appartenenze. In caso contrario, si crea il ruolo.

Creazione di `UserManager` oggetto sembra essere più complesso rispetto di `RoleManager` controllare, tuttavia è quasi identica. È codificata solo una riga anziché le diverse. In questo caso, il parametro che si sta passando viene creata l'istanza come un nuovo oggetto contenuto tra parentesi.

Creare l'utente "canEditUser" creando un nuovo `ApplicationUser` oggetto. Quindi, se si crea correttamente l'utente, aggiungere l'utente al nuovo ruolo.

> [!NOTE] 
> 
> La gestione degli errori verrà aggiornato durante l'esercitazione "Gestione di errori ASP.NET" più avanti in questa serie di esercitazioni.


Al successivo avvio dell'applicazione, l'utente denominato "canEditUser" essere aggiunto come ruolo denominato "canEdit" dell'applicazione. Più avanti in questa esercitazione verrà accesso come utente "canEditUser" per visualizzare le funzionalità aggiuntive che verranno aggiunti durante questa esercitazione. Per informazioni dettagliate API su ASP.NET Identity, vedere il [ASPNET Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Per ulteriori informazioni sull'inizializzazione del sistema di identità di ASP.NET, vedere il [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Limitare l'accesso alla pagina di amministrazione

L'applicazione di esempio Wingtip Toys consente agli utenti anonimi e gli utenti connessi visualizzare e acquistare prodotti. Tuttavia, l'utente connesso che dispone del ruolo personalizzata "canEdit" può accedere a una pagina con restrizioni per aggiungere e rimuovere i prodotti.

#### <a name="add-an-administration-folder-and-page"></a>Aggiungere una cartella di amministrazione e di una pagina

Successivamente, si creerà una cartella denominata *Admin* applicazione di esempio per l'utente "canEditUser" appartenenti al ruolo personalizzato di Wingtip Toys.

1. Fare doppio clic sul nome del progetto (**Wingtip Toys**) in **Esplora** e selezionare **Aggiungi**  - &gt; **nuova cartella**.
2. Denominare la nuova cartella *Admin*.
3. Fare doppio clic su di *Admin* cartella e quindi selezionare **Aggiungi**  - &gt; **nuovo elemento**.   
 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
4. Selezionare il **Visual c#** - &gt; **Web** gruppo di modelli a sinistra. Selezionare dall'elenco al centro, **Web Form con pagina Master**, denominarla *AdminPage.aspx***,** e quindi selezionare **Aggiungi**.
5. Selezionare il *Site. master* file della pagina master e quindi scegliere **OK**.

#### <a name="add-a-webconfig-file"></a>Aggiungere un File Web. config

Aggiungendo un *Web. config* file per il *Admin* cartella, è possibile limitare l'accesso alla pagina contenuta nella cartella.

1. Fare doppio clic su di *Admin* cartella e selezionare **Aggiungi**  - &gt; **nuovo elemento**.  
 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Nell'elenco di modelli web di Visual c#, selezionare **File di configurazione Web**dall'elenco al centro, accettare il nome predefinito di *Web. config***,** e quindi selezionare **Aggiungere**.
3. Sostituire il contenuto nel documento XML esistente di *Web. config* file con le operazioni seguenti:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Salvare il *Web. config* file. Il *Web. config* specifica di file che appartengono al ruolo "canEdit" dell'applicazione soltanto l'utente può accedere alla pagina contenuta nel *Admin* cartella.

### <a name="including-custom-role-navigation"></a>Inclusi lo spostamento di ruolo personalizzata

Per consentire all'utente del ruolo personalizzata "canEdit" passare alla sezione Amministrazione dell'applicazione, è necessario aggiungere un collegamento per il *Site. master* pagina. Solo gli utenti che appartengono al ruolo "canEdit" saranno in grado di visualizzare il **Admin** collegare e accedere alla sezione di amministrazione.

1. In Esplora soluzioni, trovare e aprire il *Site. master* pagina.
2. Per creare un collegamento per l'utente del ruolo "canEdit", aggiungere il markup evidenziato in giallo per il seguente elenco non ordinato `<ul>` elemento in modo che l'elenco viene visualizzato come segue:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Aprire il *Site.Master.cs* file. Rendere il **Admin** collegamento visibile solo all'utente "canEditUser" aggiungendo il codice evidenziato in giallo per il `Page_Load` gestore. Il `Page_Load` gestore apparirà come segue:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Quando il caricamento della pagina, il codice verifica se l'utente connesso con il ruolo di "canEdit". Se l'utente appartiene al ruolo "canEdit", l'elemento span contenente il collegamento per il *AdminPage.aspx* pagina (e conseguenza il collegamento all'interno dell'estensione) viene reso visibile.

### <a name="enabling-product-administration"></a>Abilitazione dell'amministrazione del prodotto

Finora, aver creato il ruolo "canEdit" e aggiungere un utente di "canEditUser", una cartella di amministrazione e una pagina di amministrazione. È impostato diritti di accesso per la cartella di amministrazione e la pagina e avere aggiunto un collegamento di navigazione per l'utente del ruolo "canEdit" all'applicazione. Successivamente, verrà aggiunto il markup per il *AdminPage.aspx* pagina e il codice per il *AdminPage.aspx.cs* file code-behind, per consentire all'utente con il ruolo "canEdit" aggiungere e rimuovere i prodotti.

1. In **Esplora**, aprire il *AdminPage.aspx* file dal *Admin* cartella.
2. Sostituire il codice esistente con il seguente:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Aprire quindi il *AdminPage.aspx.cs* file code-behind facendo clic con il *AdminPage.aspx* e facendo clic su **Visualizza codice**.
4. Sostituire il codice esistente nel *AdminPage.aspx.cs* file code-behind con il codice seguente:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

Nel codice che sono stati immessi per il *AdminPage.aspx.cs* file code-behind, una classe denominata `AddProducts` esegue il lavoro effettivo dell'aggiunta di prodotti nel database. Questa classe non esiste ancora, quindi verrà creato ora.

1. In **Esplora**, fare doppio clic su di *logica* cartella e quindi selezionare **Aggiungi**  - &gt; **nuovo elemento**.   
 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **codice** gruppo di modelli a sinistra. Selezionare quindi **classe**dal centro elenco e denominarlo *AddProducts.cs*.   
 Viene visualizzato il nuovo file di classe.
3. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

Il *AdminPage.aspx* consente all'utente che appartengono al ruolo "canEdit" aggiungere e rimuovere i prodotti. Quando viene aggiunto un nuovo prodotto, i dettagli sul prodotto vengono convalidati e quindi inseriti nel database. Il nuovo prodotto è immediatamente disponibile per tutti gli utenti dell'applicazione web.

#### <a name="unobtrusive-validation"></a>Convalida non intrusiva

I dettagli del prodotto che l'utente di *AdminPage.aspx* pagina vengono convalidati utilizzando i controlli di convalida (`RequiredFieldValidator` e `RegularExpressionValidator`). Questi controlli utilizzano automaticamente la convalida non intrusiva. Convalida non intrusiva consente i controlli di convalida da utilizzare JavaScript per la logica di convalida lato client, ovvero che la pagina non richiede un andata e ritorno al server da convalidare. Per impostazione predefinita, la convalida non intrusiva è incluso nel *Web. config* file in base all'impostazione di configurazione seguenti:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Espressioni regolari

Il prezzo del prodotto sul *AdminPage.aspx* pagina viene convalidata utilizzando un **RegularExpressionValidator** controllo. Questo controllo verifica se il valore di input associato (la casella di testo "AddProductPrice") corrisponde al pattern specificato dall'espressione regolare. Un'espressione regolare è una notazione di criteri di ricerca che consente di individuare rapidamente e i modelli di caratteri specifico corrisponde. Il **RegularExpressionValidator** controllo include una proprietà denominata `ValidationExpression` che contiene l'espressione regolare utilizzata per convalidare l'input di prezzo, come illustrato di seguito:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Controllo fileUpload

Oltre ai controlli di convalida dell'input e, è stato aggiunto il **FileUpload** controllo il *AdminPage.aspx* pagina. Questo controllo consente di caricare i file. In questo caso, si consente solo i file di immagine da caricare. Nel file code-behind (*AdminPage.aspx.cs*), quando il `AddProductButton` si fa clic, il codice controlla il `HasFile` proprietà del **FileUpload** controllo. Se il controllo dispone di un file e se il tipo di file (in base all'estensione) è consentito, viene salvata l'immagine per il *immagini* cartella e *immagini/miniature* cartella dell'applicazione.

#### <a name="model-binding"></a>Associazione di modelli

In precedenza in questa serie di esercitazioni associazione del modello è utilizzato per popolare un **ListView** (controllo), un **FormsView** (controllo), un **GridView** (controllo) e un  **DetailView** controllo. In questa esercitazione, utilizzare l'associazione di modelli per popolare un **DropDownList** controllo con un elenco di categorie di prodotti.

Il markup che è stato aggiunto per il *AdminPage.aspx* file contiene un **DropDownList** controllo denominato `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Associazione del modello utilizzata per popolare questo **DropDownList** impostando il `ItemType` attributo e `SelectMethod` attributo. Il `ItemType` attributo specifica che viene usato il `WingtipToys.Models.Category` digitare durante il popolamento del controllo. È definito questo tipo all'inizio di questa serie di esercitazioni creando la `Category` classe (mostrato sotto). Il `Category` classe si trova nel *modelli* cartella all'interno di *Category.cs* file.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

Il `SelectMethod` attributo del **DropDownList** controllo specifica l'uso di `GetCategories` metodo (mostrato sotto) che è incluso nel file code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Questo metodo specifica che un `IQueryable` interfaccia viene utilizzata per valutare una query su un `Category` tipo. Il valore restituito viene utilizzato per popolare il **DropDownList** nel markup della pagina (*AdminPage.aspx*).

Il testo visualizzato per ogni elemento nell'elenco viene specificato impostando il `DataTextField` attributo. Il `DataTextField` attributo viene utilizzato il `CategoryName` del `Category` (illustrato in precedenza) per visualizzare ogni categoria nella classe di **DropDownList** controllo. Il valore effettivo viene passato quando è selezionato un elemento nel **DropDownList** controllo basato sul `DataValueField` attributo. Il `DataValueField` attributo è impostato sul `CategoryID` come definire la `Category` classe (illustrato in precedenza).

### <a name="how-the-application-will-work"></a>Il funzionamento dell'applicazione

Quando l'utente che appartengono al ruolo "canEdit" passa alla pagina per la prima volta, il `DropDownAddCategory` **DropDownList** controllo venga popolato come descritto in precedenza. Il `DropDownRemoveProduct` **DropDownList** controllo viene inoltre popolato con i prodotti usando lo stesso approccio. L'utente appartenente al ruolo "canEdit" seleziona il tipo di categoria e aggiunge i dettagli sul prodotto (**nome**, **descrizione**, **prezzo**, e **Filediimmagine**). Quando l'utente che appartengono al ruolo "canEdit" seleziona il **Add Product** pulsante, il `AddProductButton_Click` gestore dell'evento viene attivato. Il `AddProductButton_Click` gestore eventi si trova nel file code-behind (*AdminPage.aspx.cs*) controlla il file di immagine per assicurarsi che corrisponda ai tipi di file consentiti *(con estensione gif*, *PNG*, *JPEG*, o *jpg*). Il file di immagine, quindi viene salvato in una cartella dell'applicazione di esempio Wingtip Toys. Successivamente, il nuovo prodotto viene aggiunto al database. Per eseguire l'aggiunta di un nuovo prodotto, una nuova istanza di `AddProducts` classe creata e denominata prodotti. Il `AddProducts` classe dispone di un metodo denominato `AddProduct`, e l'oggetto prodotti chiama questo metodo per aggiungere i prodotti nel database.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Se il codice aggiunto correttamente il nuovo prodotto per il database, la pagina viene ricaricata con il valore di stringa di query `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Quando si ricarica la pagina, la stringa di query è incluso nell'URL. Per ricaricare la pagina, l'utente che appartengono al ruolo "canEdit" visualizzare immediatamente gli aggiornamenti nel **DropDownList** ai controlli di *AdminPage.aspx* pagina. Inoltre, includendo la stringa di query con l'URL, la pagina possibile visualizzare un messaggio di conferma all'utente appartenente al ruolo "canEdit".

Quando il *AdminPage.aspx* pagina ricaricamenti, il `Page_Load` eventi viene chiamato.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Il `Page_Load` gestore eventi controlla il valore di stringa di query e determina se visualizzare un messaggio di conferma.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per vedere come è possibile aggiungere, delete e update elementi nel carrello acquisti. Il totale di carrello acquisti rifletterà il costo totale di tutti gli elementi nel carrello acquisti.

1. In Esplora soluzioni, premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 Il browser viene visualizzata e viene illustrato il *Default.aspx* pagina.
2. Fare clic su di **Accedi** collegamento nella parte superiore della pagina. 

    ![L'appartenenza e amministrazione - Accedi collegamento](membership-and-administration/_static/image2.png)

 Il *Login.aspx* viene visualizzata la pagina.
3. Utilizzare il seguente nome utente e la password:  
 Nome utente:canEditUser@wingtiptoys.com  
 Password: Pa$ $word1 

    ![L'appartenenza e amministrazione - pagina di accesso](membership-and-administration/_static/image3.png)
4. Fare clic su di **Accedi** pulsante nella parte inferiore della pagina.
5. Nella parte superiore della pagina successiva, seleziona il **Admin** collegamento per passare al *AdminPage.aspx* pagina. 

    ![L'appartenenza e amministrazione - collegamento Admin](membership-and-administration/_static/image4.png)
6. Per testare la convalida dell'input, fare clic su di **Add Product** pulsante senza aggiungere i dettagli del prodotto. 

    ![L'appartenenza e amministrazione - pagina di amministrazione](membership-and-administration/_static/image5.png)

 Si noti che vengono visualizzati i messaggi di campo obbligatorio.
7. Aggiungere i dettagli per un nuovo prodotto e quindi scegliere il **Add Product** pulsante. 

    ![L'appartenenza e amministrazione - Aggiungi prodotto](membership-and-administration/_static/image6.png)
8. Selezionare **prodotti** dal menu di spostamento superiore per visualizzare il nuovo prodotto è stato aggiunto. 

    ![L'appartenenza e amministrazione - Mostra nuovo prodotto](membership-and-administration/_static/image7.png)
9. Fare clic su di **Admin** collegamento per tornare alla pagina di amministrazione.
10. Nel **rimuovere prodotto** sezione della pagina, selezionare il nuovo prodotto è stato aggiunto nel **DropDownListBox**.
11. Fare clic su di **rimuovere prodotto** pulsante per rimuovere il prodotto di nuovo dall'applicazione. 

    ![L'appartenenza e amministrazione - Rimuovi prodotto](membership-and-administration/_static/image8.png)
12. Selezionare **prodotti** dal menu di spostamento superiore per confermare che il prodotto è stato rimosso.
13. Fare clic su **disconnettersi** esista la modalità di amministrazione.   
 Si noti che il riquadro di spostamento superiore non viene più visualizzato il **Admin** voce di menu.

## <a name="summary"></a>Riepilogo

In questa esercitazione aggiunto un ruolo personalizzato e un utente appartenente al ruolo personalizzato, un accesso limitato alla cartella di amministrazione e pagina e navigazione per l'utente che appartengono al ruolo personalizzato fornito. Associazione del modello è utilizzato per popolare un **DropDownList** controllo con i dati. È implementato il **FileUpload** controllo e i controlli di convalida. Inoltre, si è appreso come aggiungere e rimuovere i prodotti da un database. Nella prossima esercitazione, verrà illustrato come implementare il routing di ASP.NET.

## <a name="additional-resources"></a>Risorse aggiuntive

[Web. config - elemento di autorizzazione](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[Identità di ASP.NET](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Distribuire un'App di moduli Web ASP.NET in modo sicuro con appartenenza, OAuth e il Database SQL a un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[Precedente](checkout-and-payment-with-paypal.md)
[Successivo](url-routing.md)
