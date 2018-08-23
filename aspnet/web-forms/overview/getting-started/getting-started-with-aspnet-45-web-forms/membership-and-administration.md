---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Appartenenza e amministrazione | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 23d08d5a05a8321fbc794e2c9b54cc39c9b5baf6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831470"
---
<a name="membership-and-administration"></a>Appartenenza e amministrazione
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


Questa esercitazione illustra come aggiornare l'applicazione di esempio Wingtip Toys per aggiungere un ruolo personalizzato e usare ASP.NET Identity. Viene inoltre illustrato come implementare una pagina di amministrazione da cui l'utente con un ruolo personalizzato è possibile aggiungere e rimuovere i prodotti dal sito Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) è il sistema di appartenenze utilizzato per compilare un'applicazione web ASP.NET ed è disponibile in ASP.NET 4.5. ASP.NET Identity viene usato il modello di progetto Web Form di Visual Studio 2013, nonché i modelli per [ASP.NET MVC](../../../../mvc/index.md), [API Web ASP.NET](../../../../web-api/index.md), e [applicazione a pagina singola ASP.NET](../../../../single-page-application/index.md). Anche in particolare, è possibile installare il sistema di identità di ASP.NET con NuGet quando si inizia con un'applicazione Web vuota. Tuttavia, in questa serie di esercitazioni usano i **Web Form**projecttemplate, che include il sistema di identità di ASP.NET. ASP.NET Identity rende più facile l'integrazione dei dati di profili specifici dell'utente con i dati dell'applicazione. ASP.NET Identity consente, inoltre, è possibile scegliere il modello di persistenza per i profili utente nell'applicazione. È possibile archiviare i dati in un database di SQL Server o un altro archivio dati, inclusi *NoSQL* gli archivi dati, ad esempio tabelle di archiviazione Windows Azure.

Questa esercitazione si basa sull'esercitazione precedente denominata "Completamento della transazione e pagamento con PayPal" della serie di esercitazioni di Wingtip Toys.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come usare il codice per aggiungere un ruolo personalizzato e un utente all'applicazione.
- Come limitare l'accesso alla cartella di amministrazione, pagina.
- Come per la navigazione per l'utente a cui appartiene il ruolo personalizzato.
- Come usare l'associazione di modelli per popolare una [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) controllo con le categorie di prodotti.
- Come caricare un file per l'applicazione web usando il [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) controllo.
- Come usare i controlli di convalida per implementare la convalida dell'input.
- Come aggiungere e rimuovere i prodotti dall'applicazione.

## <a name="these-features-are-included-in-the-tutorial"></a>Queste funzionalità sono inclusi nell'esercitazione:

- Identità ASP.NET
- Configurazione e l'autorizzazione
- Associazione di modelli
- Convalida discreta

Web Form ASP.NET offre funzionalità di appartenenza. Usando il modello predefinito, si ottengono funzionalità di appartenenza incorporata che è possibile utilizzare immediatamente quando viene eseguita l'applicazione. Questa esercitazione illustra come usare ASP.NET Identity per aggiungere un ruolo personalizzato e assegnare un utente a tale ruolo. Si apprenderà come limitare l'accesso alla cartella administration. Si aggiungerà una pagina della cartella amministrazione che consente a un utente con un ruolo personalizzato per aggiungere e rimuovere i prodotti e visualizzare in anteprima un prodotto dopo che è stato aggiunto.

## <a name="adding-a-custom-role"></a>Aggiunta di un ruolo personalizzato

Uso di ASP.NET Identity, è possibile aggiungere un ruolo personalizzato e assegnare un utente a tale ruolo tramite il codice.

1. Nella **Esplora soluzioni**, fare clic sui *logica* cartella e creare una nuova classe.
2. Denominare la nuova classe *RoleActions.cs*.
3. Modificare il codice in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. Nelle **Esplora soluzioni**, aprire il *Global.asax.cs* file.
5. Modificare il *Global.asax.cs* file aggiungendo il codice evidenziato in giallo in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Si noti che `AddUserAndRole` è sottolineato in rosso. Fare doppio clic il codice AddUserAndRole.  
   La lettera "A" all'inizio del metodo evidenziato verrà sottolineata.
7. La lettera "A" del mouse e scegliere l'interfaccia utente che consente di generare uno stub di metodo per la `AddUserAndRole` (metodo). 

    ![L'appartenenza e Advministration - genera Stub metodo](membership-and-administration/_static/image1.png)
8. Fare clic sull'opzione intitolato:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Aprire il *RoleActions.cs* del file dalle *per la logica* cartella.  
   Il `AddUserAndRole` metodo è stato aggiunto al file della classe.
10. Modificare il *RoleActions.cs* file rimuovendo il `NotImplementedeException` e aggiungendo il codice evidenziato in giallo, in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Il codice sopra riportato innanzitutto stabilisce un contesto di database per il database di appartenenza. Il database di appartenenza viene archiviato anche un *mdf* del file nei *App\_dati* cartella. Sarà in grado di visualizzare il database dopo il primo utente ha effettuato l'accesso a questa applicazione web. 

> [!NOTE] 
> 
> Se si desidera archiviare i dati di appartenenza con i dati del prodotto, è possibile utilizzare lo stesso **DbContext** usato per archiviare i dati del prodotto nel codice precedente.


 Il *interne* (parola chiave) è un modificatore di accesso per tipi (ad esempio le classi) e i membri dei tipi (ad esempio metodi o proprietà). Tipi interni o i membri sono accessibili solo all'interno dei file contenuti nell'assembly stesso *(con estensione dll* file). Quando si compila l'applicazione, un file di assembly *(con estensione dll*) viene creato che contiene il codice che viene eseguito quando si esegue l'applicazione. 

Oggetto `RoleStore` oggetto, che fornisce la gestione dei ruoli, viene creato in base al contesto di database.

> [!NOTE] 
> 
> Si noti che durante la `RoleStore` viene creato l'oggetto utilizza un oggetto generico `IdentityRole` tipo. Ciò significa che il `RoleStore` può solo contenere `IdentityRole` oggetti. Anche con i Generics, le risorse in memoria vengono gestite meglio.


Successivamente, il `RoleManager` dell'oggetto, viene creato in base il `RoleStore` oggetto appena creato. il `RoleManager` API che consente di salvare automaticamente le modifiche a correlate al ruolo di oggetto espone il `RoleStore`. Il `RoleManager` può solo contenere `IdentityRole` oggetti perché il codice Usa il `<IdentityRole>` tipo generico.

Si chiama il `RoleExists` metodo per determinare se il ruolo "canEdit" è presente nel database delle appartenenze. In caso contrario, si creerà il ruolo.

Creazione di `UserManager` oggetto sembra essere più complessi del `RoleManager` controllare, tuttavia è quasi identica a quella. È sufficiente codificata in una riga invece di più versioni. In questo caso, il parametro che si sta passando viene creata l'istanza come un nuovo oggetto contenuto in parentesi.

Successivamente viene creato l'utente "canEditUser" creando un nuovo `ApplicationUser` oggetto. Quindi, se crei correttamente l'utente, aggiungere l'utente al nuovo ruolo.

> [!NOTE] 
> 
> La gestione degli errori verrà aggiornato durante l'esercitazione "ASP.NET Error Handling" più avanti in questa serie di esercitazioni.


Al successivo avvio dell'applicazione, l'utente denominato "canEditUser" verrà aggiunto come il ruolo "canEdit" dell'applicazione denominato. Più avanti in questa esercitazione, sarà l'accesso come utente "canEditUser" per visualizzare le funzionalità aggiuntive che si verranno aggiunti durante questa esercitazione. Per informazioni dettagliate API su ASP.NET Identity, vedere la [ASPNET Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Per altre informazioni sull'inizializzazione il sistema di identità di ASP.NET, vedere la [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Limitare l'accesso alla pagina di amministrazione

L'applicazione di esempio Wingtip Toys consente agli utenti anonimi e agli utenti registrati visualizzare e acquistare i prodotti. Tuttavia, l'utente ha eseguito l'accesso con il ruolo "canEdit" personalizzato può accedere a una pagina con restrizioni per poter aggiungere e rimuovere i prodotti.

#### <a name="add-an-administration-folder-and-page"></a>Aggiungere una cartella di amministrazione e pagina

Successivamente, si creerà una cartella denominata *Admin* applicazione di esempio per l'utente "canEditUser" che appartiene al ruolo personalizzato di Wingtip Toys.

1. Fare clic sul nome del progetto (**Wingtip Toys**) nel **Esplora soluzioni** e selezionare **Add**  - &gt; **nuova cartella**.
2. Denominare la nuova cartella *Admin*.
3. Fare doppio clic il *Admin* cartella e quindi selezionare **Add**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
4. Selezionare il <strong>Visual c#</strong> - &gt; <strong>Web</strong> gruppo di modelli a sinistra. Nell'elenco al centro, selezionare <strong>Web Form con pagina Master</strong>, denominarlo <em>AdminPage.aspx</em><strong>,</strong> e quindi selezionare <strong>Aggiungi</strong>.
5. Selezionare il *Site. master* del file della pagina master e quindi scegliere **OK**.

#### <a name="add-a-webconfig-file"></a>Aggiungere un File Web. config

Aggiungendo un *Web. config* del file per il *Admin* cartella, è possibile limitare l'accesso alla pagina contenuta nella cartella.

1. Fare doppio clic il *Admin* cartella e selezionare **Add**  - &gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Nell'elenco di modelli web di Visual c#, selezionare <strong>Web. config</strong>dall'elenco al centro, accettare il nome predefinito della <em>Web. config</em><strong>,</strong> e quindi selezionare <strong>Aggiungere</strong>.
3. Sostituire il contenuto nel codice XML esistente di *Web. config* file con il codice seguente:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Salvare il *Web. config* file. Il *Web. config* file specifica che soltanto l'utente che appartengono al ruolo "canEdit" dell'applicazione può accedere alla pagina contenuta nel *Admin* cartella.

### <a name="including-custom-role-navigation"></a>Inclusi lo spostamento di un ruolo personalizzato

Per consentire all'utente del ruolo "canEdit" personalizzato passare alla sezione Amministrazione dell'applicazione, è necessario aggiungere un collegamento per il *Site. master* pagina. Solo gli utenti che appartengono al ruolo "canEdit" saranno in grado di vedere le **Admin** collegare e accedere alla sezione di amministrazione.

1. In Esplora soluzioni, trovare e aprire il *Site. master* pagina.
2. Per creare un collegamento per l'utente del ruolo "canEdit", aggiungere il markup evidenziato in giallo nell'elenco non ordinato seguente `<ul>` elemento in modo che l'elenco viene visualizzato come segue:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Aprire il *Site.Master.cs* file. Rendere la **Admin** collegamento visibile solo all'utente "canEditUser" aggiungendo il codice evidenziato in giallo per il `Page_Load` gestore. Il `Page_Load` gestore apparirà come segue:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Quando la pagina viene caricata, il codice controlla se l'utente ha eseguito l'accesso dispone del ruolo di "canEdit". Se l'utente appartiene al ruolo "canEdit", l'elemento span contenente il collegamento per il *AdminPage.aspx* pagina (e, di conseguenza, il collegamento all'interno dell'estensione) viene reso visibile.

### <a name="enabling-product-administration"></a>Abilitazione dell'amministrazione del prodotto

Finora, aver creato il ruolo "canEdit" e aggiungere un utente di "canEditUser", una cartella di amministrazione e da una pagina di amministrazione. Si dispongano di impostare i diritti di accesso per la cartella di amministrazione e la pagina e avrà aggiunto un collegamento di navigazione per l'utente del ruolo "canEdit" all'applicazione. Successivamente, verrà aggiunto il markup per il *AdminPage.aspx* pagina e scrivere il codice per il *AdminPage.aspx.cs* file code-behind che consentirà all'utente con il ruolo "canEdit" aggiungere e rimuovere i prodotti.

1. Nella **Esplora soluzioni**, aprire il *AdminPage.aspx* del file dal *Admin* cartella.
2. Sostituire il markup esistente con il codice seguente:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Successivamente, aprire il *AdminPage.aspx.cs* file code-behind facendo clic con il *AdminPage.aspx* e scegliendo **Visualizza codice**.
4. Sostituire il codice esistente nel *AdminPage.aspx.cs* file code-behind con il codice seguente:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

Il codice immesso per il *AdminPage.aspx.cs* file code-behind, una classe denominata `AddProducts` esegue il lavoro effettivo dell'aggiunta di prodotti nel database. Questa classe non esiste ancora, in modo che si creerà ora.

1. Nella **Esplora soluzioni**, fare doppio clic il *per la logica* cartella e quindi selezionare **Add**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **codice** gruppo di modelli a sinistra. Quindi, selezionare **classe**dalla parte centrale elencare e denominarlo *AddProducts.cs*.   
   Viene visualizzato il nuovo file di classe.
3. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

Il *AdminPage.aspx* pagina consente all'utente che appartengono al ruolo "canEdit" aggiungere e rimuovere i prodotti. Quando viene aggiunto un nuovo prodotto, i dettagli sul prodotto vengono convalidati e quindi immessi nel database. Il nuovo prodotto è immediatamente disponibile per tutti gli utenti dell'applicazione web.

#### <a name="unobtrusive-validation"></a>Convalida discreta

I dettagli del prodotto fornito dall'utente sul *AdminPage.aspx* pagina vengono convalidati mediante i controlli di convalida (`RequiredFieldValidator` e `RegularExpressionValidator`). Questi controlli usano automaticamente la convalida discreta. La convalida discreta consente i controlli di convalida da usare JavaScript per la logica di convalida lato client, ovvero che la pagina non richiede una corsa al server da convalidare. Per impostazione predefinita, la convalida discreta è incluso nel *Web. config* file basato sull'impostazione di configurazione seguenti:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Espressioni regolari

Il prezzo del prodotto sul *AdminPage.aspx* pagina viene convalidata utilizzando un **RegularExpressionValidator** controllo. Questo controllo verifica che il valore di controllo di input associato (la casella di testo "AddProductPrice") corrisponde al pattern specificato dall'espressione regolare. Un'espressione regolare è una notazione di criteri di ricerca che consente di individuare rapidamente e corrispondono a modelli di caratteri specifica. Il **RegularExpressionValidator** controllo include una proprietà denominata `ValidationExpression` che contiene l'espressione regolare utilizzata per convalidare l'input di prezzo, come illustrato di seguito:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Controllo fileUpload

Oltre ai controlli di input e convalida, è stato aggiunto il **FileUpload** controllare per il *AdminPage.aspx* pagina. Questo controllo offre la possibilità di caricare i file. In questo caso, si consente solo i file di immagine da caricare. Nel file code-behind (*AdminPage.aspx.cs*), quando il `AddProductButton` fa, il codice controlla il `HasFile` proprietà del **FileUpload** controllo. Se il controllo dispone di un file e se il tipo di file (in base all'estensione) è consentito, l'immagine viene salvata per il *immagini* cartella e il *immagini/Thumbs* cartella dell'applicazione.

#### <a name="model-binding"></a>Associazione di modelli

In precedenza in questa serie di esercitazioni è stato usato l'associazione di modelli per popolare una **ListView** (controllo), un **FormsView** (controllo), una **GridView** (controllo) e un  **Un controllo DetailView** controllo. In questa esercitazione, si usa l'associazione di modelli per popolare una **DropDownList** controllo con un elenco delle categorie di prodotti.

Il markup che è stato aggiunto per il *AdminPage.aspx* file contiene una **DropDownList** controllo denominato `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Si usa l'associazione di modelli per popolare questo **DropDownList** impostando il `ItemType` attributo e il `SelectMethod` attributo. Il `ItemType` attributo specifica l'uso di `WingtipToys.Models.Category` digitare durante il popolamento del controllo. È definito questo tipo all'inizio di questa serie di esercitazioni creando il `Category` classe (mostrato sotto). Il `Category` classe si trova nel *modelli* cartella all'interno di *Category.cs* file.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

Il `SelectMethod` attributo del **DropDownList** controllo specifica l'uso di `GetCategories` metodo (mostrato sotto) che è incluso nel file code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Questo metodo specifica che un `IQueryable` interfaccia viene utilizzata per valutare una query su un `Category` tipo. Il valore restituito viene utilizzato per popolare la **DropDownList** nel markup della pagina (*AdminPage.aspx*).

Il testo visualizzato per ogni elemento nell'elenco viene specificato impostando il `DataTextField` attributo. Il `DataTextField` attributo viene utilizzato il `CategoryName` delle `Category` classe (illustrato in precedenza) per visualizzare ogni categoria nel **DropDownList** controllo. Il valore effettivo che viene passato quando viene selezionato un elemento nel **DropDownList** controllo basato sul `DataValueField` attributo. Il `DataValueField` attributo è impostato il `CategoryID` come definire nel `Category` classe (illustrato in precedenza).

### <a name="how-the-application-will-work"></a>Il funzionamento dell'applicazione

Quando l'utente che appartengono al ruolo "canEdit" passa alla pagina per la prima volta, il `DropDownAddCategory` **DropDownList** controllo venga popolato come descritto in precedenza. Il `DropDownRemoveProduct` **DropDownList** controllo viene anche popolato con i prodotti con lo stesso approccio. L'utente che appartengono al ruolo "canEdit" seleziona il tipo di categoria e aggiunge i dettagli del prodotto (**Name**, **descrizione**, **prezzo**, e **Filediimmagine**). Quando l'utente che appartengono al ruolo "canEdit" Seleziona le **Aggiungi prodotto** pulsante, il `AddProductButton_Click` gestore dell'evento viene attivato. Il `AddProductButton_Click` gestore dell'evento che si trova nel file code-behind (*AdminPage.aspx.cs*) controlla il file di immagine per assicurarsi che corrispondano ai tipi di file consentiti *(con estensione gif*, *PNG*, *JPEG*, o *jpg*). Quindi, viene salvato il file di immagine in una cartella dell'applicazione di esempio Wingtip Toys. Successivamente, il nuovo prodotto viene aggiunto al database. Per eseguire l'aggiunta di un nuovo prodotto, una nuova istanza di `AddProducts` classe viene creata e denominata products. Il `AddProducts` classe dispone di un metodo denominato `AddProduct`, e l'oggetto prodotti chiama questo metodo per aggiungere prodotti al database.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Se il codice viene aggiunto correttamente il nuovo prodotto al database, la pagina viene ricaricata con il valore di stringa di query `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Quando si ricarica la pagina, la stringa di query è incluso nell'URL. Per ricaricare la pagina, l'utente che appartengono al ruolo "canEdit" è possibile visualizzare immediatamente gli aggiornamenti nel **DropDownList** ai controlli le *AdminPage.aspx* pagina. Inoltre, includendo la stringa di query con l'URL, la pagina può visualizzare un messaggio di conferma all'utente che appartengono al ruolo "canEdit".

Quando la *AdminPage.aspx* pagina ricaricamenti, il `Page_Load` eventi viene chiamato.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Il `Page_Load` gestore eventi controlla il valore di stringa di query e determina se visualizzare un messaggio di conferma.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione ora per vedere come è possibile aggiungere, delete e update elementi nel carrello acquisti. Il totale del carrello riporterà il costo totale di tutti gli elementi nel carrello acquisti.

1. In Esplora soluzioni, premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
   Il browser si apre e Mostra le *default. aspx* pagina.
2. Scegliere il **Accedi** collegamento nella parte superiore della pagina. 

    ![L'appartenenza e amministrazione - Log nel collegamento](membership-and-administration/_static/image2.png)

   Il *aspx* viene visualizzata la pagina.
3. Usare il seguente nome utente e la password:  
   Nome utente: canEditUser@wingtiptoys.com  
   Password: Pa$$word1 

    ![Appartenenza e amministrazione - pagina di accesso](membership-and-administration/_static/image3.png)
4. Scegliere il **Accedi** pulsante nella parte inferiore della pagina.
5. Nella parte superiore della pagina successiva, selezionare la **Admin** collegamento per passare alle *AdminPage.aspx* pagina. 

    ![L'appartenenza e amministrazione: collegamento di amministrazione](membership-and-administration/_static/image4.png)
6. Per testare la convalida dell'input, selezionare la **Aggiungi prodotto** pulsante senza aggiungere i dettagli del prodotto. 

    ![Appartenenza e amministrazione - pagina di amministrazione](membership-and-administration/_static/image5.png)

   Si noti che vengono visualizzati i messaggi di campo obbligatorio.
7. Aggiungere i dettagli per un nuovo prodotto e quindi scegliere il **Aggiungi prodotto** pulsante. 

    ![Appartenenza e amministrazione - Aggiungi prodotto](membership-and-administration/_static/image6.png)
8. Selezionare **prodotti** dal menu di spostamento superiore per visualizzare il nuovo prodotto è stato aggiunto. 

    ![Appartenenza e amministrazione - prodotto nuovo Show](membership-and-administration/_static/image7.png)
9. Scegliere il **Admin** collegamento per tornare alla pagina di amministrazione.
10. Nel **rimuovere prodotto** sezione della pagina, selezionare il nuovo prodotto aggiunto nel **DropDownListBox**.
11. Scegliere il **rimuovere prodotto** pulsante per rimuovere il nuovo prodotto dall'applicazione. 

    ![Appartenenza e amministrazione - Rimuovi prodotto](membership-and-administration/_static/image8.png)
12. Selezionare **prodotti** dal menu di spostamento superiore per confermare che il prodotto è stata rimossa.
13. Fare clic su **disconnettersi** esista la modalità di amministrazione.   
    Si noti che il riquadro di spostamento in alto non viene più visualizzato il **Admin** voce di menu.

## <a name="summary"></a>Riepilogo

In questa esercitazione aggiunto un ruolo personalizzato e un utente appartenente al ruolo personalizzato, accesso limitato alla cartella di amministrazione, pagina e forniti navigazione per l'utente che appartengono al ruolo personalizzato. Associazione di modelli è utilizzato per popolare una **DropDownList** controllo con i dati. È stato implementato il **FileUpload** controllo e i controlli di convalida. Inoltre, si è appreso come aggiungere e rimuovere i prodotti da un database. Nella prossima esercitazione, verrà illustrato come implementare il routing di ASP.NET.

## <a name="additional-resources"></a>Risorse aggiuntive

[Web. config - elemento authorization](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Distribuire un'App di moduli Web ASP.NET sicura con appartenenza, OAuth e Database SQL in un sito Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Precedente](checkout-and-payment-with-paypal.md)
> [Successivo](url-routing.md)
