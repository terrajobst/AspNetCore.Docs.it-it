<!-- Options common to Razor Pages and Controller -->
| Opzione               | DESCRIZIONE|
| ----------------- | ------------ |
| --model oppure -m  | Classe di modelli da usare. |
| --dataContext oppure -dc  | Classe `DbContext` da usare. |
| --bootstrapVersion oppure -b  | Specifica la versione di bootstrap. I valori validi sono `3` e `4`. Il valore predefinito è `4`. Se necessaria e non presente, viene creata una directory *wwwroot* che include i file di bootstrap della versione specificata. |
| --referenceScriptLibraries oppure -scripts |  Librerie di script di riferimento nelle viste generate. Aggiunge `_ValidationScriptsPartial` per modificare e creare pagine. |
| --layout oppure -l | Pagina di layout personalizzata da usare. |
| --useDefaultLayout oppure -udl | Usare il layout predefinito per le viste. |
| --force oppure -f | Sovrascrivere i file esistenti. |
| --relativeFolderPath oppure -outDir | Percorso relativo della cartella di output dal progetto in cui vengono generati i file. Se non è specificato, i file vengono generati nella cartella del progetto. |