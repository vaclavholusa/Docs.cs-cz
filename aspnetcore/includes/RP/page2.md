`<form method="post">` Element je [pomocná značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocník značku formuláře automaticky zahrne [antiforgery token](xref:security/anti-request-forgery).

Modul generování uživatelského rozhraní vytvoří kódu Razor pro každé pole v modelu (s výjimkou ID) podobný následujícímu:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Ověření značky Pomocníci](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` a ` <span asp-validation-for`) zobrazit chyby ověření. Ověření je podrobněji popsány dále v této série.

[Popisek značky pomocná](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje popisek popisek a `for` atribut pro `Title` vlastnost.

[Vstupní značka pomocná](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) používá [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atributů a atributů HTML, které jsou potřebné pro architekturu jQuery ověření na straně klienta vytváří.