* Startup.cs: [třída při spuštění](../fundamentals/startup.md) -třída nakonfiguruje kanál požadavku, která zpracovává všechny požadavky vytvořené pro aplikaci.
* Program.cs: [třídy Program](../fundamentals/index.md) obsahující hlavní vstupní bod aplikace.
* firstapp.csproj: [soubor projektu](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj) formát souboru projektu nástroje MSBuild pro aplikace ASP.NET Core. Obsahuje odkazy na projekt na projekt, odkazů NuGet a další související položky projektu.
* appSettings.JSON určený / appsettings. Development.JSON: Prostředí základní aplikace nastavení konfigurační soubor. [Najdete v části konfigurace](xref:fundamentals/configuration/index).
* bower.JSON: Bower závislosti balíčků pro projekt.
* .bowerrc: bower konfigurační soubor, který definuje vhodného místa pro instalaci součástí při stahování Bower prostředky.
* bundleconfig.JSON: konfigurační soubory pro sdružování a zmenšování front-endové prostředky JavaScript a CSS.
* Zobrazení: Obsahuje zobrazení syntaxe Razor. Zobrazení jsou komponenty, které zobrazují aplikace uživatelské rozhraní (UI). Obecně platí toto uživatelské rozhraní zobrazí data modelu.
* Řadiče: Obsahuje řadiče MVC, původně *HomeController.cs*. Řadiče jsou třídy, které zpracovávají požadavky prohlížeče.
* Wwwroot: kořenovou složku webové aplikace.

Další informace najdete v části [vzor MVC](xref:mvc/overview).
