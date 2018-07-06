---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web Pages (Razor) riferimento rapido sulle API | Microsoft Docs
author: tfitzmac
description: Questa pagina contiene un elenco con brevi esempi di oggetti più comunemente usati, proprietà e metodi per la programmazione di ASP.NET Web Pages con sintassi Razor.
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5fb5b8d29fce25fb36704a58b673f5f3bd582873
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836765"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Web Pages (Razor) riferimento rapido sulle API
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa pagina contiene un elenco con brevi esempi di oggetti più comunemente usati, proprietà e metodi per la programmazione di ASP.NET Web Pages con sintassi Razor.
> 
> Le descrizioni contrassegnate con "(v2)" sono state introdotte in ASP.NET Web Pages versione 2.
> 
> Per la documentazione di riferimento API, vedere la [documentazione di riferimento di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=208659) su MSDN.
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione funziona anche con ASP.NET Web Pages 2 e ASP.NET Web Pages 1.0 (tranne le funzionalità contrassegnate v2).


Questa pagina contiene informazioni di riferimento per le operazioni seguenti:

- [Classi](#Classes)
- [Dati](#Data)
- [Helper](#Helpers)
- [Convalida](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Classi

### `AppState[key], AppState[index],App`

Contiene dati che possono essere condiviso da tutte le pagine nell'applicazione. È possibile usare dinamica `App` proprietà a cui accedere agli stessi dati, come nell'esempio seguente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Converte un valore stringa in un valore booleano (true/false). Restituisce false o il valore specificato se la stringa non rappresenta true o false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Converte un valore stringa a data/ora. Restituisce `DateTime.MinValue` o il valore specificato se la stringa non rappresenta una data/ora.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Converte un valore stringa in un valore decimale. Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Converte un valore stringa in un valore float. Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Converte un valore stringa in un intero. Restituisce 0 o il valore specificato se la stringa non rappresenta un numero intero.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Crea un URL compatibile con browser da un percorso file locale, con le parti di percorso aggiuntivi facoltativi.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Esegue il rendering *valore* come markup HTML anziché eseguirne il rendering come codificata in formato HTML di output.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Restituisce true se il valore può essere convertito da una stringa nel tipo specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Restituisce true se l'oggetto o una variabile non ha alcun valore.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Restituisce true se la richiesta viene pubblicato un POST. (Le richieste iniziali sono in genere è un'operazione GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Specifica il percorso di una pagina di layout da applicare a questa pagina.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contiene i dati condivisi tra la pagina, pagine di layout e pagine parziali nella richiesta corrente. È possibile usare dinamica `Page` proprietà a cui accedere agli stessi dati, come nell'esempio seguente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Pagine di layout) Visualizza il contenuto di una pagina di contenuto che non si trova in tutte le sezioni denominate.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Esegue il rendering di una pagina di contenuto utilizzando il percorso specificato e dati aggiuntivi facoltativi. È possibile ottenere i valori dei parametri aggiuntivi da `PageData` con posizione (ad esempio, 1) o una chiave (ad esempio, 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Pagine di layout) Esegue il rendering di una sezione di contenuto con un nome. Impostare *obbligatorio* su false per rendere una sezione facoltativa.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Ottiene o imposta il valore di un cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Ottiene i file che sono stati caricati nella richiesta corrente.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Ottiene i dati che è stati registrati in un form (come stringhe). `Request[key]` controlla sia il `Request.Form` e il `Request.QueryString` raccolte.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Ottiene i dati che è stati specificati nella stringa di query dell'URL. `Request[key]` controlla sia il `Request.Form` e il `Request.QueryString` raccolte.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Convalida per un elemento del form, il valore di stringa di query, dal cookie o valore dell'intestazione della richiesta in modo selettivo viene disabilitato. Convalida delle richieste è abilitata per impostazione predefinita e impedisce agli utenti di inserimento markup o altro contenuto potenzialmente pericoloso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Aggiunge un'intestazione HTTP del server nella risposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Memorizza nella cache l'output delle pagine per un determinato periodo. Facoltativamente, impostare *scorrevole* reimpostare il timeout per ogni accesso alla pagina e *varyByParams* per memorizzare nella cache diverse versioni della pagina per ogni stringa di query diversi nella richiesta della pagina.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Reindirizza la richiesta del browser in una nuova posizione.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Imposta il codice di stato HTTP inviato al browser.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Scrive il contenuto del *dati* alla risposta con un tipo MIME facoltativo.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Scrive il contenuto di un file alla risposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Pagine di layout) Definisce una sezione di contenuto con un nome.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Decodifica una stringa che viene codificato in formato HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codifica una stringa per il rendering nel markup HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Restituisce il percorso fisico del server per il percorso virtuale specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Decodifica il testo da un URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codifica testo da inserire in un URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Ottiene o imposta un valore che è presente finché l'utente chiude il browser.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Visualizza una rappresentazione di stringa del valore dell'oggetto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Ottiene i dati aggiuntivi da URL (ad esempio, *MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Modifica la password per l'utente specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Conferma di un account usando il token di conferma di account.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Crea un nuovo account utente con il nome utente specificato e la password. Per richiedere un token di conferma, passare true per *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Ottiene l'identificatore integer per l'utente attualmente connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Ottiene il nome per l'utente attualmente connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Genera un token di reimpostazione della password che è possibile inviare tramite posta elettronica a un utente in modo che l'utente può reimpostare la password.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Restituisce l'ID utente da nome utente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Restituisce true se l'utente corrente è connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Restituisce true se l'utente è stata confermata (ad esempio, tramite un messaggio di conferma).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Restituisce true se il nome dell'utente corrente corrisponde al nome utente specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Accesso all'utente in impostando un token di autenticazione nel cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Accesso all'utente di disconnettersi rimuovendo il cookie di token di autenticazione.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Se l'utente non è autenticato, imposta lo stato HTTP su 401 (non autorizzato).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Se l'utente corrente non è un membro di uno dei ruoli specificati, imposta lo stato HTTP su 401 (non autorizzato).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Se l'utente corrente non è l'utente specificato da *username*, imposta lo stato HTTP su 401 (non autorizzato).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Se il token di reimpostazione della password è valido, cambia la password dell'utente per la nuova password.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Dati

### `Database.Execute(SQLstatement [,parameters]`

Viene eseguito *SQLstatement* (con parametri facoltativi), ad esempio INSERT, DELETE o UPDATE e restituisce il numero di record interessati.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Restituisce la colonna identity della riga inserita più di recente.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Apre il file di database specificato o il database specificato usando una stringa di connessione denominata dal *Web. config* file.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Consente di aprire un database usando la stringa di connessione. (Ciò è in contrasto con `Database.Open`, che usa un nome di stringa di connessione.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Esegue una query del database utilizzando *SQLstatement* (passando facoltativamente i parametri) e restituisce i risultati sotto forma di raccolta.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Viene eseguito *SQLstatement* (con parametri facoltativi) e restituisce un singolo record.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Viene eseguito *SQLstatement* (con parametri facoltativi) e restituisce un valore singolo.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Helper

### `Analytics.GetGoogleHtml(webPropertyId)`

Esegue il rendering del codice JavaScript di Analitica di Google per l'ID specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Esegue il rendering di codice JavaScript di Analitica StatCounter per il progetto specificato.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Esegue il rendering di codice JavaScript di Analitica Yahoo per l'account specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Passa una ricerca di Bing. Per specificare il sito alla ricerca e un titolo per la casella di ricerca, è possibile impostare il `Bing.SiteUrl` e `Bing.SiteTitle` proprietà. In genere è impostare queste proprietà  *\_AppStart* pagina.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inizializza un grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Aggiunge una legenda in un grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Aggiunge una serie di valori per il grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Restituisce un hash per i dati specificati. L'algoritmo predefinito è `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Consente agli utenti di Facebook di stabilire una connessione a pagine.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Esegue il rendering dell'interfaccia utente per il caricamento di file.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Esegue il rendering del tag del giocatore di Xbox specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Esegue il rendering di immagini Gravatar per l'indirizzo di posta elettronica specificato.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Converte un oggetto dati in una stringa in formato JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Converte una stringa di input con codifica JSON in un oggetto dati che è possibile eseguire l'iterazione su o inserire in un database.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Esegue il rendering di collegamenti di rete basati su social network con il titolo specificato e l'URL facoltativi.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Associa un messaggio di errore a un campo del form. Usare il `ModelState` helper per accedere a questo membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Associa un messaggio di errore a un form. Usare il `ModelState` helper per accedere a questo membro.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Restituisce true se non sono presenti errori di convalida. Usare il `ModelState` helper per accedere a questo membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Esegue il rendering le proprietà e valori di un oggetto e qualsiasi figlio oggetti.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Esegue il rendering di test di verifica reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Imposta le chiavi pubbliche e private per il servizio di reCAPTCHA. In genere è impostare queste proprietà  *\_AppStart* pagina.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Restituisce il risultato del test di reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Esegue il rendering informazioni sullo stato su ASP.NET Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Esegue il rendering di un flusso di Twitter per l'utente specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Esegue il rendering di un flusso di Twitter per il testo di ricerca specificati.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Esegue il rendering di un lettore video Flash per il file specificato con l'altezza e larghezza facoltativa.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Esegue il rendering di un lettore multimediale di Windows per il file specificato con l'altezza e larghezza facoltativa.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Esegue il rendering di un lettore Silverlight per l'oggetto specificato *XAP* file con l'altezza e larghezza necessaria.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Restituisce l'oggetto specificato da *chiave*, oppure null se l'oggetto non viene trovato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Rimuove l'oggetto specificato da *chiave* dalla cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Inserisce *valore* nella cache con il nome specificato da *chiave*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Crea un nuovo `WebGrid` usando i dati da una query dell'oggetto.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Esegue il rendering di markup per la visualizzazione dei dati in una tabella HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Esegue il rendering di un pager per il `WebGrid` oggetto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Carica un'immagine dal percorso specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Aggiunge l'immagine specificata come filigrana.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Aggiunge il testo specificato all'immagine.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Consente di capovolgere l'immagine orizzontalmente o verticalmente.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Carica un'immagine quando viene inserita un'immagine di una pagina durante il caricamento di un file.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Ridimensiona un'immagine.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Consente di ruotare l'immagine a sinistra o destra.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Salva l'immagine nel percorso specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Imposta la password per il server SMTP. In genere è impostare questa proprietà  *\_AppStart* pagina.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Invia un messaggio di posta elettronica.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Imposta il nome del server SMTP. In genere è impostare questa proprietà<em>\_AppStart</em> pagina.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Imposta il nome utente per il server SMTP. In genere è necessario impostare questa proprietà  *\_AppStart* pagina.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Convalida

### `Html.ValidationMessage(field)`

(v2) Esegue il rendering di un messaggio di errore di convalida per il campo specificato.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Visualizza un elenco di tutti gli errori di convalida.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Registra un elemento di input utente per il tipo di convalida specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) In modo dinamico viene eseguito il rendering degli attributi di classe CSS per la convalida lato client in modo che è possibile formattare i messaggi di errore di convalida. (Richiede che si fa riferimento le librerie di script client appropriato e definire le classi CSS).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Abilita la convalida lato client per il campo di input utente. (Richiede di fare riferimento a librerie di script client appropriato).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Restituisce true se tutti gli elementi input utente che vengono registrate per la convalida contengono i valori validi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Specifica che gli utenti devono fornire un valore per l'elemento di input utente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Specifica che gli utenti devono fornire valori per ognuno degli elementi di input utente. Questo metodo consente di specificare un messaggio di errore personalizzato.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Specifica un test di convalida quando si usa il `Validation.Add` (metodo).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
