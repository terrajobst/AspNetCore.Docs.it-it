---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Creazione di un livello di accesso ai dati (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà avviare sin dall'inizio e creare Data Access Layer (DAL), usando i dataset tipizzati, accedere alle informazioni in un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e124edc137db8444c687092f33db7e03aaeb179
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398320"
---
<a name="creating-a-data-access-layer-c"></a>Creazione di un livello di accesso ai dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) o [Scarica il PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> In questa esercitazione verrà avviare sin dall'inizio e creare Data Access Layer (DAL), usando i dataset tipizzati, accedere alle informazioni in un database.


## <a name="introduction"></a>Introduzione

Gli sviluppatori web nostre vite ruotano intorno utilizzo dei dati. Creiamo database per archiviare i dati, il codice per recuperare e apportarvi modifiche e le pagine web per raccogliere e riepilogarla. Questa è la prima esercitazione di una serie di lunga durata che verranno illustrate le tecniche di implementazione di questi modelli comuni di ASP.NET 2.0. Si inizierà con la creazione di un [architettura software](http://en.wikipedia.org/wiki/Software_architecture) composte di una Data Access Layer (DAL) utilizzo dei dataset tipizzati, un livello di logica di Business (BLL) che verranno applicate le regole di business personalizzata e un livello di presentazione composto da ASP.NET le pagine che condividere un layout di pagina comuni. Una volta poste presupposti questo back-end, verrà spostato nella creazione di report, che illustra come visualizzare, riepilogare, raccogliere e convalidare i dati da un'applicazione web. Queste esercitazioni sono pensate per essere concise e vengono fornite istruzioni dettagliate con numerose catture di schermata che illustra il processo in modo visivo. Ogni esercitazione è disponibile nelle versioni di Visual Basic e c# e comprende un download del codice completo utilizzato. (Questa prima esercitazione di è abbastanza lunga, ma il resto vengono presentati in base a blocchi molto più semplice).

Per queste esercitazioni si userà una versione di Microsoft SQL Server 2005 Express Edition del database Northwind inserito nel **App\_dati** directory. Oltre al file di database, il **App\_dati** cartella contiene anche gli script SQL per la creazione del database, nel caso in cui si vuole usare una versione di database diverso. Questi script possono anche essere [scaricati direttamente da Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), se si preferisce. Se si usa una versione diversa di SQL Server del database Northwind, è necessario aggiornare il **stringa NORTHWNDConnectionString** l'impostazione dell'applicazione **Web. config** file. L'applicazione web è stato creato usando Visual Studio 2005 Professional Edition come un file di progetto di sito Web basato su sistema. Tuttavia, tutte le esercitazioni funzioneranno altrettanto bene con la versione gratuita di Visual Studio 2005, [in Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
In questa esercitazione verrà avviare sin dall'inizio e creare la Data Access Layer (DAL), seguita dalla creazione il livello per la logica di Business (BLL) nella seconda esercitazione e l'utilizzo nel layout di pagina e la navigazione nel terzo. Le esercitazioni dopo il terzo verrà si basano le basi di cui le prime tre. Abbiamo molti trattati in questa prima esercitazione, quindi avvia Visual Studio prima di iniziare.

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Passaggio 1: Creazione di un progetto Web e la connessione al Database

Prima di poter creare la Data Access Layer (DAL), è innanzitutto necessario creare un sito web e configurare il database. Iniziare creando un nuovo file in base al sistema sito web ASP.NET. A tale scopo, passare al menu File e scegliere Nuovo sito Web, la finestra di dialogo Nuovo sito Web. Scegliere il modello di sito Web ASP.NET, impostare l'elenco di riepilogo a discesa percorso al File System, scegliere una cartella in cui inserire il sito web e impostare il linguaggio c#.


[![Creare un nuovo sito di Web File basate sul sistema](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Figura 1**: creare un sito Web New File System-Based ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image3.png))


Verrà creato un nuovo sito web con un **default. aspx** pagina ASP.NET e un **App\_dati** cartella.

Con il sito web creato, il passaggio successivo consiste nell'aggiungere un riferimento al database in Esplora Server di Visual Studio. Tramite l'aggiunta di un database in Esplora Server è possibile aggiungere tabelle, stored procedure, viste e così via tutto da Visual Studio. È anche possibile visualizzare i dati della tabella o creare query personalizzate manualmente o graficamente tramite il generatore delle Query. Inoltre, quando si compila il set di dati tipizzato per DAL è necessario a punto di Visual Studio per il database da cui deve essere costruito i set di dati tipizzato. Anche se è possibile fornire queste informazioni di connessione a quel punto nel tempo, Visual Studio popola automaticamente un elenco di riepilogo a discesa dei database di cui è già registrato in Esplora Server.

La procedura per aggiungere il database Northwind in Esplora Server dipende dal fatto che si desidera utilizzare il database di SQL Server 2005 Express Edition nel **App\_dati** cartella o se si dispone di Microsoft SQL Server 2000 o 2005 impostazione di server di database che si desidera utilizzare in alternativa.

## <a name="using-a-database-in-theappdatafolder"></a>Uso di un Database in theApp\_DataFolder

Se non è SQL Server 2000 o 2005 database server per connettersi a, o semplicemente si desidera evitare di dover aggiungere il database a un server di database, è possibile usare la versione di SQL Server 2005 Express Edition del database Northwind cui si trova il preesistente scaricato e's **App\_Data** cartella (**NORTHWND. File MDF**).

Un database inseriti nel **App\_dati** cartella viene aggiunto automaticamente a Esplora Server. Presupponendo che siano installati nel computer SQL Server 2005 Express Edition dovrebbe essere un nodo denominato NORTHWND. File MDF in Esplora Server, che è possibile espandere ed esplorare le tabelle, viste, stored procedure e così via (vedere la figura 2).

Il **App\_Data** cartella può contenere anche Microsoft Access **mdb** file, che, analogamente alle relative controparti di SQL Server, vengono aggiunti automaticamente a Esplora Server. Se non si desidera usare una delle opzioni di SQL Server, è sempre possibile [scaricare una versione di Microsoft Access del file di database Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) trascinamento della selezione nel **App\_dati** directory. Tenere presente, tuttavia, che non sono i database di Access come ricco di funzionalità di SQL Server e non sono progettati per essere usata negli scenari di sito web. Inoltre, un paio delle esercitazioni 35 + userà alcune funzionalità a livello di database che non sono supportate per l'accesso.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>La connessione al Database in un Server di Database Microsoft SQL Server 2000 o 2005

In alternativa, è possibile connettersi a un database di Northwind installato in un server di database. Se il server di database non ha già installato il database Northwind, è innanzitutto necessario aggiungerlo al server di database eseguendo lo script di installazione incluso nel download dell'esercitazione o da [scaricando la versione di SQL Server 2000 di Northwind lo script di installazione e](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) direttamente dal sito web Microsoft.

Dopo aver installato il database, passare a Esplora Server in Visual Studio, fare clic sul nodo Connessioni dati e quindi scegliere Aggiungi connessione. Se non è visibile Esplora Server aprire la visualizzazione / Esplora Server o hit Ctrl + Alt + S. Verrà visualizzata la finestra di dialogo Aggiungi connessione, in cui è possibile specificare il server a cui connettersi, le informazioni di autenticazione e il nome del database. Dopo avere configurato le informazioni di connessione del database e fatto clic sul pulsante OK, il database verrà aggiunto come nodo sotto il nodo Connessioni dati. È possibile espandere il nodo del database per esplorare le tabelle, viste, stored procedure e così via.


![Aggiungere una connessione al database Northwind del Server di Database](creating-a-data-access-layer-cs/_static/image4.png)

**Figura 2**: aggiungere una connessione al database Northwind del Server di Database


## <a name="step-2-creating-the-data-access-layer"></a>Passaggio 2: Creazione di livello di accesso ai dati

Quando si lavora con una delle opzioni dei dati consiste nell'incorporare la logica specifica i dati direttamente nel livello di presentazione (in un'applicazione web, la compongono il livello di presentazione nella pagine ASP.NET). L'operazione potrebbe richiedere il modulo di scrittura di codice ADO.NET in parte di codice della pagina ASP.NET o mediante il controllo SqlDataSource dalla parte di markup. In entrambi i casi, questo approccio è strettamente collegato la logica di accesso ai dati con il livello di presentazione. L'approccio consigliato, tuttavia, consiste nel separare la logica di accesso ai dati dal livello di presentazione. A questo livello separato viene definito il livello di accesso ai dati, per brevità e viene in genere implementato come un progetto libreria di classi separato. I vantaggi di questa architettura a più livelli sono ben documentati (vedere la sezione "Ulteriori letture" alla fine di questa esercitazione per informazioni su questi vantaggi) è l'approccio verrà usato in questa serie.

Tutto il codice specifico per l'origine dati sottostante, ad esempio la creazione di una connessione al database, emissione **selezionate**, **Inserisci**, **UPDATE**, e  **Elimina** comandi e così via devono trovarsi nel DAL. Il livello di presentazione non deve contenere tutti i riferimenti a tale codice di accesso ai dati, ma debba invece effettuare chiamate al per le richieste di tutti i dati. Livelli di accesso ai dati in genere contengono i metodi per l'accesso ai dati del database sottostante. Il database Northwind, ad esempio, ha **prodotti** e **categorie** tabelle che registrano i prodotti in vendita e le categorie a cui appartengono. Nel nostro DAL abbiamo metodi, ad esempio:

- **GetCategories(),** che restituirà informazioni su tutte le categorie
- **GetProducts()**, che restituirà informazioni su tutti i prodotti
- **GetProductsByCategoryID (*categoryID*)**, che restituirà tutti i prodotti che appartengono a una categoria specificata
- **GetProductByProductID (*productID*)**, che restituirà informazioni su un prodotto specifico

Questi metodi, quando richiamata, verranno connettersi al database, eseguire la query appropriata e restituire i risultati. Come è possibile tornare questi risultati è importante. Questi metodi può semplicemente restituire un set di dati o un DataReader popolata dalla query sul database, ma in teoria questi risultati devono essere restituiti utilizzando *oggetti fortemente tipizzati*. Un oggetto fortemente tipizzato è uno cui schema è definito rigidamente in fase di compilazione, mentre l'opposto, un oggetto debolmente tipizzato, è uno schema di cui non è noto fino al runtime.

Ad esempio, il DataReader e DataSet (per impostazione predefinita) sono oggetti non fortemente tipizzato, perché i relativi schemi sono definito per le colonne restituite dalla query database utilizzata per popolarli. Accedere a una particolare colonna di una DataTable non fortemente tipizzato, è necessario usare una sintassi come:  <strong><em>DataTable</em>. Righe [<em>indice</em>] ["<em>nomecolonna</em>"]</strong>. Il DataTable implicato comunemente dalla tipizzazione in questo esempio è esposto dal fatto che è necessario accedere al nome di colonna utilizzando una stringa o un indice ordinale. Un oggetto DataTable fortemente tipizzato, d'altra parte, avrà ciascuna delle relative colonne implementate come proprietà, generando codice simile a:  <strong><em>DataTable</em>. Righe [<em>indice</em>]. *columnName</strong>*.

Per restituire oggetti fortemente tipizzati, gli sviluppatori possono creare i propri oggetti business personalizzati o usano i dataset tipizzati. Un oggetto business viene implementato dallo sviluppatore come rappresenta una classe le cui proprietà riflettono in genere le colonne della tabella di database sottostante l'oggetto business. Un set di dati tipizzato è una classe generata automaticamente da Visual Studio basate su uno schema di database e i cui membri sono fortemente tipizzate in base a questo schema. Il set di dati tipizzato stesso è costituito da classi che estendono le classi ADO.NET DataSet, DataTable e DataRow. Oltre a DataTable fortemente tipizzato, dataset tipizzati ora includono anche gli oggetti TableAdapter, che sono classi con metodi per la compilazione di oggetti DataTable del set di dati e la propagazione delle modifiche all'interno di oggetti DataTable nel database.

> [!NOTE]
> Per altre informazioni sui vantaggi e svantaggi dell'utilizzo di dataset tipizzati e oggetti business personalizzati, consultare [la progettazione di componenti livello dati e il passaggio attraverso i livelli dati](https://msdn.microsoft.com/library/ms978496.aspx).


Si userà i set di dati fortemente tipizzato per l'architettura di queste esercitazioni. Figura 3 viene illustrato il flusso di lavoro tra i diversi livelli di un'applicazione che usa i set di dati tipizzato.


[![Tutti i codice di accesso ai dati è necessario DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Figura 3**: è necessario codice di accesso di tutti i dati DAL ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Creazione di un DataSet tipizzato e un adattatore di tabella

Per iniziare a creare DAL nostro, si inizia aggiungendo un set di dati tipizzato al nostro progetto. A tale scopo, fare doppio clic sul nodo del progetto in Esplora soluzioni e scegliere Aggiungi un nuovo elemento. Selezionare l'opzione set di dati dall'elenco dei modelli e denominarla **Northwind.xsd**.


[![Scegliere di aggiungere un nuovo set di dati al progetto](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Figura 4**: scegliere di aggiungere un nuovo set di dati a un progetto ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image10.png))


Dopo aver fatto clic Add, quando viene richiesto di aggiungere il set di dati per il **App\_codice** cartella, scegliere Sì. Verrà quindi visualizzata la finestra di progettazione per il set di dati tipizzati e la configurazione guidata TableAdapter verrà avviato, che consente di aggiungere il primo TableAdapter al set di dati tipizzato.

Viene usato un set di dati tipizzato come una raccolta fortemente tipizzata di dati. si è costituito da istanze DataTable fortemente tipizzato, ognuno dei quali a sua volta è costituito da istanze di DataRow fortemente tipizzato. Si creerà un oggetto DataTable fortemente tipizzato per ognuna delle tabelle di database sottostanti che è necessario utilizzare in questa serie di esercitazioni. Iniziamo con la creazione di un oggetto DataTable per il **prodotti** tabella.

Tenere presente che DataTable fortemente tipizzate non includono le informazioni sull'accesso ai dati dalla tabella di database sottostante. Per poter recuperare i dati per popolare l'oggetto DataTable, viene usata una classe TableAdapter, che funziona come il livello di accesso ai dati. Per i **prodotti** DataTable, TableAdapter conterrà i metodi **GetProducts()**, **GetProductByCategoryID (*categoryID*)** e così via che è possibile richiamare dal livello di presentazione. Ruolo del DataTable è come gli oggetti fortemente tipizzati utilizzati per passare dati tra i livelli.

La configurazione guidata TableAdapter inizia con cui viene richiesto di selezionare il database da usare. L'elenco a discesa Mostra i database in Esplora Server. Se non è stato aggiunto il database Northwind in Esplora Server, è possibile fare clic sul pulsante nuova connessione al momento di farlo.


[![Scegliere il Database Northwind nell'elenco a discesa](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Figura 5**: scegliere il Northwind Database nell'elenco a discesa ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image13.png))


Dopo aver selezionato il database e fare clic su Avanti, viene chiesto se si desidera salvare la stringa di connessione nel **Web. config** file. Salvando la stringa di connessione si eviteranno averlo rigido codificati nelle classi TableAdapter, che semplifica le cose, se le informazioni sulla stringa di connessione viene modificata in futuro. Se si decide di salvare la stringa di connessione nel file di configurazione si trova il **&lt;connectionStrings&gt;** sezione, che può essere [facoltativamente crittografato](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) migliorato sicurezza o modificati in un secondo momento tramite la nuova pagina delle proprietà ASP.NET 2.0 nello strumento di amministrazione GUI IIS, che è più idonea per gli amministratori.


[![Salvare la stringa di connessione in Web. config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Figura 6**: Salva stringa di connessione **Web. config** ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image16.png))


Successivamente, è necessario definire lo schema per la prima DataTable fortemente tipizzato e fornire il primo metodo per i TableAdapter da utilizzare per popolare il set di dati fortemente tipizzati. Questi due passaggi vengono eseguiti contemporaneamente mediante la creazione di una query che restituisce le colonne della tabella che si desidera siano indicati nei nostri DataTable. Al termine della procedura guidata viene fornito un nome di metodo per questa query. Una volta che è stato eseguito, questo metodo può essere richiamato dal nostro livello presentazione. Il metodo Esegui la query definita e popolare un DataTable fortemente tipizzato.

Per iniziare a definire la query SQL che viene prima di tutto necessario indicato come si desidera eseguire la query TableAdapter. È possibile utilizzare un'istruzione SQL ad hoc, creare una nuova stored procedure o usare una stored procedure esistente. Per queste esercitazioni useremo istruzioni SQL ad hoc. Fare riferimento a [Brian Noyes](http://briannoyes.net/)dell'articolo, [creare un livello di accesso ai dati con set di dati di progettazione di Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) per un esempio di utilizzo delle stored procedure.


[![Eseguire query sui dati usando un'istruzione SQL Ad Hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Figura 7**: eseguire query sui dati usando un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image19.png))


A questo punto è possibile digitare la query SQL manualmente. Quando si crea il primo metodo in TableAdapter in genere consigliabile fare in modo che la query restituisca le colonne che devono essere espressi in DataTable corrispondente. Permette di eseguire questa mediante la creazione di una query che restituisce tutte le colonne e tutte le righe dal **prodotti** tabella:


[![Immettere la Query SQL nella casella di testo](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Figura 8**: immettere la Query SQL in the Textbox ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image22.png))


In alternativa, usare il generatore delle Query e creare graficamente la query, come illustrato nella figura 9.


[![Creare la Query in formato grafico tramite l'Editor di Query](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Figura 9**: creare la Query in formato grafico tramite l'Editor di Query ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image25.png))


Dopo aver creato la query, ma prima di procedere nella schermata successiva, fare clic sul pulsante Opzioni avanzate. Nei progetti di sito Web "istruzioni genera Insert, Update e Delete" sono l'unica opzione selezionata per impostazione predefinita; avanzata Se si esegue questa procedura guidata da un progetto Windows o una libreria di classi verrà selezionato anche l'opzione "Usa concorrenza ottimistica". Lasciare deselezionata l'opzione "Usa concorrenza ottimistica" per il momento. Esamineremo la concorrenza ottimistica in esercitazioni future.


[![Selezionare solo le genera istruzioni Insert, Update e Delete istruzioni opzione](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Figura 10**: selezionare solo le genera istruzioni Insert, Update e Delete istruzioni opzione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image28.png))


Dopo aver verificato le opzioni avanzate, fare clic su Avanti per passare alla schermata finale. Qui ci viene richiesto di selezionare i metodi da aggiungere all'oggetto TableAdapter. Sono disponibili due modelli per il popolamento dei dati:

- **Riempi un DataTable** con questo approccio viene creato un metodo che accetta un oggetto DataTable come parametro e lo popola in base ai risultati della query. La classe DataAdapter di ADO.NET, ad esempio, implementa questo modello con relativi **Fill** (metodo).
- **Restituisci un DataTable** con questo approccio il metodo crea e inserisce l'oggetto DataTable per l'utente e lo restituisce come valore restituiscono di metodi.

È possibile avere TableAdapter implementare uno o entrambi questi modelli. È inoltre possibile rinominare i metodi forniti di seguito. È possibile lasciare entrambe le caselle di controllo è selezionate, anche se si userà solo il modello di quest'ultimo in queste esercitazioni. Inoltre, è possibile rinominare il piuttosto generici **GetData** metodo **GetProducts**.

Se selezionata, la casella di controllo finale, "GenerateDBDirectMethods", viene creato **Insert ()**, **Update ()**, e **Delete ()** metodi per l'oggetto TableAdapter. Se si lascia deselezionata questa opzione, tutti gli aggiornamenti dovrà essere eseguita tramite sole dell'oggetto TableAdapter **Update ()** metodo che accetta del DataSet tipizzato, un oggetto DataTable, DataRow singola o una matrice di DataRow. (Se è stata deselezionata la "genera Insert, Update e Delete istruzioni" opzione questa casella di controllo le proprietà avanzate nella figura 9 impostazione non avrà alcun effetto.) È possibile lasciare questa casella di controllo selezionata.


[![Modificare il nome del metodo da GetData a GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Figura 11**: modificare il nome del metodo da **GetData** al **GetProducts** ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image31.png))


Completare la procedura guidata, fare clic su Fine. Dopo la chiusura della procedura guidata si viene restituiti alla finestra di progettazione set di dati che mostra l'oggetto DataTable che appena creato. È possibile visualizzare l'elenco di colonne di **prodotti** DataTable (**ProductID**, **ProductName**e così via), nonché i metodi del  **ProductsTableAdapter** (**Fill** e **GetProducts()**).


[![Il DataTable dei prodotti e ProductsTableAdapter sono stati aggiunti al set di dati tipizzati](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Figura 12**: il **prodotti** DataTable e **ProductsTableAdapter** sono stati aggiunti al set di dati tipizzato ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image34.png))


A questo punto è disponibile un set di dati tipizzato con DataTable singola (**Northwind.Products**) e una classe fortemente tipizzata DataAdapter (**NorthwindTableAdapters.ProductsTableAdapter**) con un  **GetProducts()** (metodo). Questi oggetti sono utilizzabile per accedere a un elenco di tutti i prodotti dal codice, ad esempio:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Questo codice non richiede di scrivere un bit di codice specifici di accesso ai dati. Non è stato necessario creare un'istanza di tutte le classi ADO.NET, è necessario non fare riferimento a tutte le stringhe di connessione, le query SQL, o stored procedure. Al contrario, oggetto TableAdapter fornisce il codice di accesso ai dati di basso livello per noi.

Ogni oggetto usato in questo esempio viene inoltre fortemente tipizzati, che consente di Visual Studio per fornire IntelliSense e controllo dei tipi in fase di compilazione. E meglio di tutte le DataTable restituite dal TableAdapter può essere associato a dati ASP.NET controlli Web, ad esempio GridView, DetailsView, DropDownList, CheckBoxList e molti altri. L'esempio seguente illustra l'associazione di DataTable restituito dal **GetProducts()** metodo in un controllo GridView in appena della scarsità tre righe di codice all'interno di **pagina\_carico** gestore dell'evento.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Viene visualizzato l'elenco di prodotti in un oggetto GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Figura 13**: viene visualizzato l'elenco di prodotti in un controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image37.png))


Anche se in questo esempio viene richiesto che scriviamo tre righe di codice nella nostra pagina ASP.NET **pagina\_carico** gestore dell'evento, in futuro verrà esaminato come utilizzare ObjectDataSource per recuperare i dati, in modo dichiarativo le esercitazioni di DAL. Con ObjectDataSource è non sarà necessario scrivere alcun codice e verrà visualizzato anche il supporto di impaginazione e ordinamento.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Passaggio 3: Aggiunta di parametri di metodi a livello di accesso ai dati

A questo punto nostri **ProductsTableAdapter** classe quanto include un metodo, ovvero **GetProducts()**, che restituisce tutti i prodotti nel database. Mentre la possibilità di lavorare con tutti i prodotti è indubbiamente utile, vi sono casi quando si desidera recuperare informazioni su un prodotto specifico o tutti i prodotti che appartengono a una determinata categoria. Per aggiungere tale funzionalità il livello di accesso ai dati è possibile aggiungere i metodi con parametri all'oggetto TableAdapter.

È possibile aggiungere il **GetProductsByCategoryID (*categoryID*)** (metodo). Per aggiungere un nuovo metodo di DAL, torna alla finestra di progettazione set di dati, fare doppio clic nella **ProductsTableAdapter** sezione e scegliere Aggiungi Query.


![Pulsante destro del mouse sull'oggetto TableAdapter e scegliere Aggiungi Query](creating-a-data-access-layer-cs/_static/image38.png)

**Figura 14**: pulsante destro del mouse sull'oggetto TableAdapter e scegliere Aggiungi Query


È prima di tutto verrà richiesto se si vuole accedere al database tramite un'istruzione SQL ad hoc o una stored procedure nuova o esistente. È possibile scegliere di usare nuovamente un'istruzione SQL ad hoc. Successivamente, ci viene richiesto il tipo di query SQL che desideriamo utilizzare. Poiché si vuole restituire tutti i prodotti che appartengono a una categoria specificata, è opportuno scrivere un **seleziona** istruzione che restituisce righe.


[![Scegliere di creare un'istruzione SELECT che restituisce righe](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Figura 15**: scegliere di creare un **selezionate** istruzione che restituisce righe ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image41.png))


Il passaggio successivo consiste nel definire la query SQL usata per accedere ai dati. Poiché si vogliono restituire solo i prodotti che appartengono a una determinata categoria, usare lo stesso <strong>selezionate</strong> istruzione dal <strong>GetProducts()</strong>, ma aggiungere il codice seguente <strong>dove</strong> clausola: <strong>in cui CategoryID = @CategoryID</strong> . Il <strong>@CategoryID</strong> per la configurazione guidata TableAdapter parametro indica che il metodo stiamo creando richiede un parametro di input del tipo corrispondente (vale a dire, un numero intero che ammette valori null).


[![Immettere una Query per restituire solo i prodotti in una categoria specifica](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Figura 16**: immettere una Query per restituire solo i prodotti in una categoria specificata ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image44.png))


Nel passaggio finale è possibile scegliere che i modelli da utilizzare, nonché di personalizzare i nomi dei metodi generati di accesso ai dati. Per il motivo di riempimento, è possibile modificare il nome in <strong>FillByCategoryID</strong> e per il valore restituito un oggetto DataTable restituito modello (il <strong>ottenere*X</strong>*  metodi), è possibile usare  <strong>GetProductsByCategoryID</strong>.


[![Scegliere i nomi per i metodi TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Figura 17**: scegliere i nomi per i metodi TableAdapter ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image47.png))


Dopo aver completato la procedura guidata, la finestra di progettazione set di dati include i nuovi metodi TableAdapter.


![I prodotti ora può eseguire una query per categoria](creating-a-data-access-layer-cs/_static/image48.png)

**Figura 18**: The prodotti possono ora eseguire una query per categoria


Si consiglia di aggiungere un **GetProductByProductID (*productID*)** metodo usando la stessa tecnica.

Queste query con parametri possono essere testate direttamente dalla finestra di progettazione set di dati. Fare clic sul metodo nell'oggetto TableAdapter e scegliere i dati di anteprima. Successivamente, immettere i valori da usare per i parametri e fare clic su Anteprima.


[![Vengono visualizzati tali prodotti appartenenti alla categoria di bevande](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Figura 19**: vengono visualizzati quelli prodotti appartenenti alla categoria Beverages ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image51.png))


Con il **GetProductsByCategoryID (*categoryID*)** nostro DAL metodo, possiamo creare una pagina ASP.NET che consente di visualizzare solo i prodotti in una categoria specificata. L'esempio seguente illustra tutti i prodotti della categoria Beverages, che hanno una **CategoryID** pari a 1.

Beverages.ASP

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Vengono visualizzati i prodotti della categoria Beverages](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Figura 20**: vengono visualizzati quelli prodotti della categoria Beverages ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Passaggio 4: Inserimento, aggiornamento ed eliminazione dei dati

Sono disponibili due modelli comunemente usati per l'inserimento, aggiornamento ed eliminazione dei dati. Il primo modello, che chiameremo il modello diretta database, comporta la creazione di metodi che, quando richiamata, problema un' **inserire**, **UPDATE**, o **Elimina** comando per il database che opera su un singolo record di database. Tali metodi vengono in genere passati in una serie di valori scalari (numeri interi, stringhe, valori booleani, DateTimes e così via) che corrispondono ai valori da inserire, aggiornare o eliminare. Ad esempio, con questo modello per il **prodotti** tabella il metodo delete richiederebbe un parametro intero, che indica il **ProductID** del record da eliminare, mentre il metodo insert impiegherebbe un stringa per il **ProductName**, un decimale per il **UnitPrice**, un numero intero per il **UnitsOnStock**e così via.


[![Ogni inserimento, aggiornamento e richiesta di eliminazione viene inviato al Database immediatamente](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Figura 21**: viene inviato ogni inserimento, aggiornamento e richiesta di eliminazione per i Database immediatamente ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image57.png))


L'altro modello, si farà riferimento come batch aggiornare modello, si aggiorna un intero set di dati, DataTable o raccolta di DataRow in una chiamata al metodo. Con questo modello gli sviluppatori eliminano, inserimenti e modifica il DataRow in DataTable e quindi passa tali DataRow o DataTable in un metodo di aggiornamento. Quindi questo metodo enumera il DataRow passato, determina se è state modificate, aggiunti o eliminate (tramite il DataRow [RowState proprietà](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) valore) e invia la richiesta di database appropriato per ogni record.


[![Tutte le modifiche vengono sincronizzate con il Database quando viene richiamato il metodo di aggiornamento](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Figura 22**: tutte le modifiche vengono sincronizzate con il Database quando viene richiamato il metodo di aggiornamento ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter Usa il modello di aggiornamento batch per impostazione predefinita, ma supporta anche il modello di direct DB. Dal momento che è selezionato l'opzione "genera Insert, Update e Delete di istruzioni" da the Advanced Properties quando si crea l'oggetto TableAdapter, il **ProductsTableAdapter** contiene un' **Update ()** , metodo che implementa il pattern di aggiornamento batch. In particolare, l'oggetto TableAdapter contiene un' **Update ()** metodo che può essere passato del DataSet tipizzato, fortemente tipizzato DataTable o DataRow uno o più. Se si lascia la casella di controllo "GenerateDBDirectMethods" selezionata quando prima creazione dell'oggetto TableAdapter il modello di direct DB verrà anche implementata mediante **Insert ()**, **Update ()**, e **Delete)**  metodi.

Entrambi modelli di modifica dei dati usano dell'oggetto TableAdapter **InsertCommand**, **UpdateCommand**, e **DeleteCommand** proprietà rilasciare loro **INSERT** , **UPDATE**, e **eliminare** comandi al database. È possibile esaminare e modificare il **InsertCommand**, **UpdateCommand**, e **DeleteCommand** proprietà facendo clic su TableAdapter in Progettazione DataSet e quindi passando Nella finestra Proprietà. (Assicurarsi di aver selezionato l'oggetto TableAdapter e che il **ProductsTableAdapter** oggetto corrisponde a quello selezionato nell'elenco a discesa nella finestra Proprietà.)


[![Il TableAdapter ha UpdateCommand, InsertCommand e DeleteCommand proprietà](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Figura 23**: contiene l'oggetto TableAdapter **InsertCommand**, **UpdateCommand**, e **DeleteCommand** proprietà ([fare clic su visualizzazione ingrandita immagine](creating-a-data-access-layer-cs/_static/image63.png))


Per esaminare o modificare una di queste proprietà di comando di database, fare clic sui **CommandText** sottoproprietà, che attiverà il generatore delle Query.


[![Configurare l'INSERT, UPDATE e istruzioni DELETE nel generatore di Query](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Figura 24**: configurare il **inserire**, **UPDATE**, e **eliminare** istruzioni nel generatore di Query ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image66.png))


Esempio di codice seguente viene illustrato come utilizzare il modello di aggiornamento batch a raddoppiare il prezzo di tutti i prodotti che non sono obsolete e che hanno 25 unità a magazzino o meno:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Il codice seguente viene illustrato come usare il modello di direct DB a livello di codice eliminare un determinato prodotto, quindi aggiornare uno e quindi aggiungerne uno nuovo:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Creazione personalizzata inserimento, aggiornamento ed eliminazione di metodi

Il **Insert ()**, **Update ()**, e **Delete ()** metodi creati tramite il metodo diretto di database possono essere poco pratico, in particolare per tabelle con molte colonne. Esaminando l'esempio di codice precedente, senza della Guida di IntelliSense non è particolarmente chiaro cosa **prodotti** esegue il mapping di colonna della tabella per ogni parametro di input per il **Update ()** e **Insert)**  metodi. Può accadere quando si vuole solo aggiornare una singola colonna o a due, oppure da un oggetto personalizzato **Insert ()** metodo che verrà, ad esempio, restituire il valore del record appena inserito **identità** (incremento automatico) campo.

Per creare un metodo personalizzato di questo tipo, tornare alla finestra di progettazione set di dati. Fare doppio clic sull'oggetto TableAdapter e scegliere Aggiungi Query, che restituisce per la configurazione guidata TableAdapter. Nella seconda schermata è possibile indicare il tipo di query da creare. Creiamo un metodo che aggiunge un nuovo prodotto e quindi restituisce il valore del record appena aggiunto **ProductID**. Pertanto, scegliere di creare un **Inserisci** query.


[![Creare un metodo per aggiungere una nuova riga alla tabella Products](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Figura 25**: creare un metodo per aggiungere una nuova riga per il **prodotti** tabella ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image69.png))


Nella schermata successiva il **InsertCommand**del **CommandText** viene visualizzata. Aumentare la query aggiungendo **seleziona ambito\_IDENTITY()** alla fine della query, che restituirà l'ultimo valore identity inserito in un **identità** colonna nello stesso ambito. (Vedere la [documentazione tecnica](https://msdn.microsoft.com/library/ms190315.aspx) per altre informazioni sui **ambito\_IDENTITY()** e il motivo per cui probabile che si desideri [utilizzare ambito\_IDENTITY() anziché @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Assicurarsi di terminare la **inserire** istruzione con un punto e virgola prima di aggiungere il **selezionare** istruzione.


[![Aumentare la Query per restituire il valore di SCOPE_IDENTITY)](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Figura 26**: aumentare la Query per restituire il **ambito\_IDENTITY()** valore ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image72.png))


Infine, denominare il nuovo metodo **InsertProduct**.


[![Impostare il nuovo nome di metodo su InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Figura 27**: impostare il nuovo nome del metodo **InsertProduct** ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image75.png))


Quando si torna alla finestra di progettazione set di dati si noterà che il **ProductsTableAdapter** contiene un nuovo metodo **InsertProduct**. Se questo nuovo metodo non ha un parametro per ogni colonna il **prodotti** tabella, è probabile che si è dimenticato di terminare la **Inserisci** istruzione con un punto e virgola. Configurare il **InsertProduct** (metodo) e assicurarsi di disporre di un punto e virgola che delimita il **Inserisci** e **selezionare** istruzioni.

Per impostazione predefinita, inserire i metodi di query non problema metodi, vale a dire che restituiscono il numero di righe interessate. Tuttavia, è necessario il **InsertProduct** per restituire il valore restituito dalla query, non il numero di righe interessate. A tale scopo, modificare il **InsertProduct** del metodo **ExecuteMode** proprietà **scalari**.


[![Modificare la proprietà ExecuteMode per scalare](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Figura 28**: modifica il **ExecuteMode** proprietà **scalari** ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image78.png))


Il codice seguente illustra questa nuova **InsertProduct** metodo in azione:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Passaggio 5: Completare il livello di accesso ai dati

Si noti che il **ProductsTableAdapters** classe restituisce il **CategoryID** e **SupplierID** i valori dal **prodotti** tabella, ma non include il **CategoryName** colonna dalle **categorie** tabella o la **CompanyName** colonna dal **Suppliers**di tabella, anche se probabilmente sono le colonne da visualizzare quando vengono visualizzate informazioni sul prodotto. È possibile potenziare metodo iniziale dell'oggetto TableAdapter, **GetProducts()**, per includere sia il **CategoryName** e **CompanyName** valori delle colonne, che aggiorneranno il DataTable fortemente tipizzato per includere anche queste nuove colonne.

Questo può rappresentare un problema, tuttavia, come metodi dell'oggetto TableAdapter per l'inserimento, aggiornamento, e l'eliminazione dei dati si basano su questo metodo iniziale. Per fortuna, i metodi generati automaticamente per inserimento, aggiornamento ed eliminazione non sono interessati da sottoquery nel **seleziona** clausola. Occupandosi di aggiungere la query al **categorie** e **Suppliers** come sottoquery, anziché **JOIN** s, si eviterà di dover rielaborare i metodi per la modifica dei dati. Fare clic sui **GetProducts()** metodo nella **ProductsTableAdapter** e scegliere Configura. Quindi, modificare il **seleziona** clausola in modo che risulti come:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Aggiornare l'istruzione SELECT per il metodo GetProducts()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Figura 29**: aggiornamento di **selezionare** istruzione per il **GetProducts()** (metodo) ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image81.png))


Dopo aver aggiornato il **GetProducts()** metodo da usare questa nuova query DataTable includerà due nuove colonne: **CategoryName** e **SupplierName**.


![Nel DataTable dei prodotti dispone di due nuove colonne](creating-a-data-access-layer-cs/_static/image82.png)

**Figura 30**: il **prodotti** DataTable ha due nuove colonne


Si consiglia di aggiornare il **selezionate** clausola nel **GetProductsByCategoryID (*categoryID*)** anche metodo.

Se si aggiorna il **GetProducts()** **seleziona** usando **JOIN** sintassi la finestra di progettazione set di dati non sarà in grado di generare automaticamente i metodi per l'inserimento, aggiornamento ed eliminazione dati del database usando il modello di direct DB. Al contrario, sarà necessario crearle manualmente molto come abbiamo fatto con il **InsertProduct** metodo precedentemente in questa esercitazione. Inoltre, manualmente è possibile fornire il **InsertCommand**, **UpdateCommand**, e **DeleteCommand** valori della proprietà se si desidera utilizzare il modello di aggiornamento batch.

## <a name="adding-the-remaining-tableadapters"></a>Aggiunge gli oggetti TableAdapter rimanenti

Fino ad ora, abbiamo esaminato solo uso di un singolo TableAdapter per una singola tabella di database. Tuttavia, il database di Northwind contiene più tabelle correlate che è necessario lavorare con nella nostra applicazione web. Un set di dati tipizzato può contenere più di DataTable correlati. Per completare il di conseguenza, è necessario aggiungere DataTable per le altre tabelle che verrà usato in queste esercitazioni. Per aggiungere un nuovo TableAdapter a un DataSet tipizzato, aprire la finestra di progettazione set di dati, fare doppio clic nella finestra di progettazione e scegliere Aggiungi / TableAdapter. Ciò crea un nuovo DataTable e TableAdapter e consentono di eseguire la procedura guidata che si sono esaminate in precedenza in questa esercitazione.

Richiedere alcuni minuti per creare i seguenti oggetti TableAdapter e i metodi utilizzando le query seguenti. Si noti che le query nella **ProductsTableAdapter** includono le sottoquery per recuperare i nomi di categoria e il fornitore di ogni prodotto. Inoltre, se sono state eseguite, è già stato aggiunto il **ProductsTableAdapter** della classe **GetProducts()** e **GetProductsByCategoryID (*categoryID* )** metodi.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![La finestra di progettazione set di dati dopo che sono stati aggiunti i quattro oggetti TableAdapter](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Figura 31**: il set di dati finestra di progettazione dopo il quattro oggetti TableAdapter sono stati aggiunti ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Aggiungere codice personalizzato al DAL

Il TableAdapter e DataTable aggiunto al set di dati tipizzati sono espressi come un file di XML Schema Definition (**Northwind.xsd**). È possibile visualizzare queste informazioni sullo schema facendo clic sui **Northwind.xsd** file in Esplora soluzioni e scegliere Visualizza codice.


[![Il File XML Schema Definition (XSD) per l'Employees DataSet tipizzato](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Figura 32**: File di XML Schema Definition (XSD) per il set di dati tipizzato Employees ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image88.png))


Queste informazioni sullo schema viene tradotta in codice c# o Visual Basic in fase di progettazione durante la compilazione o in fase di esecuzione (se necessario), a questo punto è possibile eseguire attraverso di esso con il debugger. Per visualizzare questo codice generato automaticamente Vai a Visualizzazione classi e il drill down per le classi TableAdapter o set di dati tipizzato. Se non è disponibile la classe sullo schermo, andare al menu View e selezionarlo da qui o premere Ctrl + Maiusc + C. Dalla visualizzazione classi è possibile visualizzare le proprietà, metodi ed eventi delle classi TableAdapter e DataSet tipizzato. Per visualizzare il codice per un metodo specifico, fare doppio clic sul nome del metodo in visualizzazione classi o fare clic su di esso e scegliere Vai a definizione.


![Esaminare il codice generato automaticamente, selezionare Vai alla definizione da Visualizzazione classi](creating-a-data-access-layer-cs/_static/image89.png)

**Figura 33**: esaminare il codice generato automaticamente, selezionare Vai alla definizione da Visualizzazione classi


Codice generato automaticamente può essere una di risparmiare molto tempo, il codice è spesso molto generico e deve essere personalizzato per soddisfare le esigenze di un'applicazione. Il rischio di estensione di codice generato automaticamente, tuttavia, è che lo strumento che ha generato il codice potrebbe decidere che è il momento di "Rigenera" e sovrascrivere le personalizzazioni. Con il concetto di classe parziale nuovo .NET 2.0, è facile suddividere una classe su più file. Ciò ci consente di aggiungere i propri metodi, proprietà ed eventi per le classi generate automaticamente senza doversi preoccupare di sovrascrivere i personalizzazioni Visual Studio.

Per illustrare come personalizzare DAL, aggiungiamo un **GetProducts()** metodo per il **SuppliersRow** classe. Il **SuppliersRow** classe rappresenta un singolo record nelle **Suppliers** tabella; ogni provider can supplier zero a molti prodotti, in modo **GetProducts()** restituirà quelli prodotti di fornitore specificato. Per eseguire questo crea un nuovo file di classe nel **App\_codice** cartella denominata **SuppliersRow.cs** e aggiungere il codice seguente:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Questa classe parziale indica al compilatore che, quando la compilazione la **Northwind.SuppliersRow** classe per includere il **GetProducts()** metodo definita. Se si compila il progetto e quindi tornare alla visualizzazione classi si noterà **GetProducts()** ora elencato come un metodo dei **Northwind.SuppliersRow**.


![Il metodo GetProducts() fa ora parte della classe Northwind.SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**Figura 34**: il **GetProducts()** metodo fa ora parte del **Northwind.SuppliersRow** classe


Il **GetProducts()** metodo ora può essere utilizzato per enumerare il set di prodotti per un particolare fornitore, come illustrato nel codice seguente:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Questo tipo di dati può essere visualizzato anche in una qualsiasi di ASP. Controlli Web dei dati di NET. La pagina seguente usa un controllo GridView con due campi:

- Un BoundField che visualizza il nome di ogni fornitore, e
- Un TemplateField contenente un controllo BulletedList associato ai risultati restituiti per il **GetProducts()** metodo per ogni fornitore.

Verrà esaminato come visualizzare tali relazioni master / dettaglio in esercitazioni future. Per ora, in questo esempio è progettato per illustrare l'utilizzo del metodo personalizzato aggiunto al **Northwind.SuppliersRow** classe.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Nome della società del fornitore è elencato nella colonna sinistra relativi prodotti in destra](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Figura 35**: nome della società il fornitore è elencato nella colonna sinistra, relativi prodotti in destra ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Riepilogo

Quando compila un'applicazione web di creazione deve essere uno dei primi passaggi, che si verificano prima di iniziare a creare il livello di presentazione. Con Visual Studio, creazione di DAL basato su dataset tipizzati è un'attività che può essere eseguita in 10-15 minuti senza dover scrivere una riga di codice. Le esercitazioni in futuro verranno si basano su DAL. Nel [prossima esercitazione](creating-a-business-logic-layer-cs.md) verrà definito un numero di regole di business e informazioni su come implementarli in un livello di logica di Business separato.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Creazione di DAL usando TableAdapter fortemente tipizzati e DataTable in Visual Studio 2005 e ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Progettazione di componenti livello dati e passare dati tramite i livelli](https://msdn.microsoft.com/library/ms978496.aspx)
- [Creare un livello di accesso ai dati con set di dati di progettazione di Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [La crittografia delle informazioni di configurazione in ASP.NET 2.0 Applications](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Panoramica degli oggetti TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Utilizzo di un DataSet tipizzato](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Usando l'accesso ai dati fortemente tipizzati in Visual Studio 2005 e ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Come estendere i metodi TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Il recupero di dati scalare da una Stored Procedure](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione in video su argomenti contenuti in questa esercitazione

- [Livelli di accesso ai dati nelle applicazioni ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Come associare manualmente un set di dati a un controllo Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Come lavorare con i set di dati e i filtri di un'applicazione ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Carlos Santos, Abel Gomez, Liz Shulok, Dennis Patterson, Hilton Giesenow e Ron Green. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](creating-a-business-logic-layer-cs.md)
