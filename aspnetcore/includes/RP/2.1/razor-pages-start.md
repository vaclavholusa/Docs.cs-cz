Vytvoří výchozí šablony **RazorPagesMovie**, **Domů**, **o** a **kontaktujte** odkazy a stránky. V závislosti na velikosti okna prohlížeče možná budete muset klikněte na ikonu navigace zobrazíte odkazy.

![Index nebo Domovská stránka](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Otestujte odkazy. **RazorPagesMovie** a **Domů** odkazy přejít na stránku indexu. **o** a **kontaktujte** odkazy přejít na `About` a `Contact` stránky v uvedeném pořadí.

## <a name="project-files-and-folders"></a>Soubory projektu a složek

Následující tabulka uvádí soubory a složky v projektu. V tomto kurzu *Startup.cs* soubor je nejdůležitější pochopit. Není nutné prozkoumat všechny odkazy níže uvedenou. Odkazy jsou uvedeny jako odkaz, pokud budete potřebovat další informace o souboru nebo složce v projektu.

| Soubor nebo složka | Účel |
| -------------- | ------- |
| *wwwroot* | Obsahuje statické prostředky. V tématu [statické soubory](xref:fundamentals/static-files). |
| *Stránky* | Složka pro [stránky Razor](xref:razor-pages/index). |
| *appsettings.json* | [Konfigurace](xref:fundamentals/configuration/index) |
| *Program.cs* | Nakonfiguruje [hostitele](xref:fundamentals/host/index) aplikace ASP.NET Core. |
| *Startup.cs* | Nakonfiguruje služby a kanál požadavku. V tématu [spuštění](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>Stránky a sdílených složek

*_Layout.cshtml* soubor obsahuje společné elementy HTML (skriptů a stylů odkazů) a nastaví rozložení pro aplikaci. Například když vyberete **RazorPagesMovie**, **Domů**, **o** nebo **obraťte se na**, zobrazí se společnou sadu elementů na webové stránce. Společné prvky patří navigační nabídce v horní části a hlavičky v dolní části okna. Další informace najdete v tématu [rozložení](xref:mvc/views/layout).

*_ValidationScriptsPartial.cshtml* soubor obsahuje odkaz na [jQuery](https://jquery.com/) ověření skripty. Když `Create` a `Edit` stránky jsou přidány později v tomto kurzu *_ValidationScriptsPartial.cshtml* soubor je používán.

*_CookieConsentPartial.cshtml* soubor poskytuje navigačním panelu a obsahu pro shrnutí ochrany osobních údajů a souborů cookie použít zásady. Další informace o prostředcích GDPR zahrnutý v projektu, najdete v části [podporu Evropa obecné Data Protection nařízení (GDPR) v ASP.NET Core)](xref:security/gdpr).

### <a name="the-pages-folder"></a>Složka stránek

*Soubor _ViewStart.cshtml* nastaví stránky Razor `Layout` vlastnost na používání *_Layout.cshtml* souboru. V tématu [rozložení](xref:mvc/views/layout) Další informace.

*_ViewImports.cshtml* soubor obsahuje direktivy Razor, které jsou importovány do každé stránce Razor. V tématu [import sdílených direktivy](xref:mvc/views/layout#importing-shared-directives) Další informace.

`About`, `Contact` a `Index` stránky jsou základní stránky můžete použít ke spuštění aplikace. `Error` Stránky se používá k zobrazení informací o chybách.
