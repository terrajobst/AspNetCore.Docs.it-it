L'elemento `<form method="post">` è un [helper tag del modulo](xref:mvc/views/working-with-forms#the-form-tag-helper). L'helper tag del modulo include automaticamente un [token antifalsificazione](xref:security/anti-request-forgery).

Il motore di scaffolding crea un markup Razor per ogni campo nel modello (tranne l'ID) simile al seguente:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

Gli [helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` e ` <span asp-validation-for`) visualizzano gli errori di convalida. Il tema della convalida viene trattato in modo più dettagliato successivamente in questa serie.

L'[helper tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera la didascalia dell'etichetta e l'attributo `for` per la proprietà `Title`.

L'[helper tag di input](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa gli attributi [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client.
