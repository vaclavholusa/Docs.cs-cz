Vytvoří výchozí šablony **RazorPagesMovie**, **Domů**, **o** a **kontaktujte** odkazy a stránky. V závislosti na velikosti okna prohlížeče možná budete muset klikněte na ikonu navigace zobrazíte odkazy.

![Index nebo Domovská stránka](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

Otestujte odkazy. **RazorPagesMovie** a **Domů** odkazy přejít na stránku indexu. **o** a **kontaktujte** odkazy přejít na `About` a `Contact` stránky v uvedeném pořadí.

## <a name="project-files-and-folders"></a>Soubory projektu a složek

Následující tabulka uvádí soubory a složky v projektu. V tomto kurzu *Startup.cs* soubor je nejdůležitější pochopit. Není nutné prozkoumat všechny odkazy níže uvedenou. Odkazy jsou uvedeny jako odkaz, pokud budete potřebovat další informace o souboru nebo složce v projektu.

| Soubor nebo složka              | Účel |
| ----------------- | ------------ | 
| Wwwroot | Obsahuje statické soubory. V tématu [statické soubory](xref:fundamentals/static-files). |
| Stránky | Složka pro [stránky Razor](xref:mvc/razor-pages/index). | 
| *appsettings.json* | [Konfigurace](xref:fundamentals/configuration/index) |
| *Program.cs* | [Hostitelé](xref:fundamentals/host/index) aplikace ASP.NET Core.|
| *Startup.cs* | Nakonfiguruje služby a kanál požadavku. V tématu [spuštění](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Složka stránek

*_Layout.cshtml* soubor obsahuje společné elementy HTML (skriptů a šablon) a nastaví rozložení pro aplikaci. Například po kliknutí na **RazorPagesMovie**, **Domů**, **o** nebo **kontaktujte**, najdete v části stejné prvky. Společné prvky patří navigační nabídce v horní části a hlavičky v dolní části okna. V tématu [rozložení](xref:mvc/views/layout) Další informace.

*Soubor _ViewStart.cshtml* nastaví stránky Razor `Layout` vlastnost na používání *_Layout.cshtml* souboru. V tématu [rozložení](xref:mvc/views/layout) Další informace.

*_ViewImports.cshtml* soubor obsahuje direktivy Razor, které jsou importovány do každé stránce Razor. V tématu [import sdílených direktivy](xref:mvc/views/layout#importing-shared-directives) Další informace.

*_ValidationScriptsPartial.cshtml* soubor obsahuje odkaz na [jQuery](https://jquery.com/) ověření skripty. Když přidáme `Create` a `Edit` stránky později v tomto kurzu *_ValidationScriptsPartial.cshtml* soubor se použije.

`About`, `Contact` a `Index` stránky jsou základní stránky můžete použít ke spuštění aplikace. `Error` Stránky se používá k zobrazení informací o chybách.